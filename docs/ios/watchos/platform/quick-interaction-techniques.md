---
title: Techniques d’Interaction rapide pour watchOS 3 dans Xamarin
description: Cet article aborde les techniques d’interaction rapide Apple a ajouté dans watchOS 3 et comment les implémenter dans Xamarin.iOS pour Apple Watch.
ms.prod: xamarin
ms.assetid: 26697F68-AF7E-4A36-988F-85E2674A4DD1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 5086724b565fb95274c4988ca1b6e4bb11064575
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61082266"
---
# <a name="quick-interaction-techniques-for-watchos-3-in-xamarin"></a>Techniques d’Interaction rapide pour watchOS 3 dans Xamarin

_Cet article aborde les techniques d’interaction rapide Apple a ajouté dans watchOS 3 et comment les implémenter dans Xamarin.iOS pour Apple Watch._

Les interactions utilisateur rapide en fournissant sont essentielles pour créer des applications attractives Apple Watch et les Complications. Nouveau watchOS 3, Apple a ajouté la prise en charge pour les modules de reconnaissance de mouvement, accès à la couronne numériques et de nouvelles techniques de Notification utilisateur et la navigation. Ceci, ainsi que la prise en charge ajoutée pour SceneKit et SpriteKit, permettent au développeur de créer facilement des interfaces riches et glanceable qui sont à la fois rapide et réactif.

## <a name="what-are-quick-interactions"></a>Quelles sont les Interactions rapide

Pour un développeur qui sert à créer des applications pour iOS ou macOS (la quantité de temps pendant lequel l’utilisateur interagit avec l’application est exprimée en minutes ou heures), la conception d’une application réussie pour l’Apple Watch peut être un défi et nécessite un autre approche.

Dans watchOS, l’utilisateur souhaite généralement déclencher leur poignet, rapidement interagir avec une application (généralement pour brève quelques secondes), puis supprimer leur poignet et continuer qui était le sien que qu’ils ont été faites.

Voici quelques exemples d’interactions rapides classiques sur l’Apple Watch :

- Démarre un minuteur.
- Vérification de la météo.
- Marquage d’un élément hors d’une liste de tâches.

Pour atteindre ces objectifs, une application sur l’Apple Watch doit être :

- **Glanceable** -ce qui signifie que, avec un rapide aperçu de l’utilisateur doit être en mesure d’obtenir les informations dont ils ont besoin. 
- **Actionnables** -ce qui signifie que les utilisateurs doit-elle être en mesure de prendre des décisions rapides et éclairées.
- **Réactif** -ce qui signifie que l’utilisateur ne doit jamais attendre de recevoir les informations dont ils ont besoin ou pour obtenir de l’action, ils le souhaitent.

### <a name="quick-interactions-length"></a>Longueur des Interactions rapide

En raison de la nature glanceable des applications de l’Apple Watch, Apple suggère que la longueur idéale d’une interaction rapide doit être de deux secondes ou moins. À la suite de cette limite de deuxième deux, le développeur devra passent beaucoup de temps à la fois conception et implémentation d’une application Apple Watch. 

## <a name="new-watchos-3-features-and-apis"></a>Nouvelle watchOS 3 fonctionnalités et API

Apple a ajouté plusieurs nouvelles fonctionnalités et API à WatchKit pour aider le développeur à ajouter des interactions rapides à leurs applications Apple Watch :

- watchOS 3 fournit l’accès aux nouveaux types d’entrée comme utilisateur :
    - Modules de reconnaissance de mouvement
    - Rotation de couronne numérique 
- watchOS 3 fournit de nouvelles façons d’afficher et de la mise à jour d’informations, telles que :
    - Navigation améliorée dans les tables
    - Nouvelle prise en charge de framework Notification utilisateur
    - Intégration de SpriteKit et SceneKit

En implémentant ces nouvelles fonctionnalités, le développeur peut s’assurer que leur application watchOS 3 est Glanceable, exploitable et réactif.

### <a name="gesture-recognizer-support"></a>Prise en charge du module de reconnaissance de mouvement

Si le développeur a implémenté les modules de reconnaissance de mouvement dans iOS, ils doivent être très bien connaître les modules de reconnaissance de mouvement dans watchOS 3. Pour actualiser, modules de reconnaissance de mouvement sont des objets qui analysent les événements tactiles de bas niveau dans des gestes reconnaissables, prédéfinis.

watchOS 3 prendra en charge le quatre modules de reconnaissance de mouvement suivantes :

- Types de mouvements discrètes :
    - Le mouvement de balayage (`WKSwipeGestureRecognizer`).
    - Le geste d’appui (`WKTapGestureRecognizer`).
- Types de mouvement continu :
    - Le mouvement panoramique (`WKPanGestureRecognizer`).
    - Le mouvement de Long-Press (`WKLongPressGestureRecognizer`).

Pour implémenter un des nouveaux modules de reconnaissance de mouvement, simplement faire glisser sur une aire de conception dans le concepteur dans Visual Studio iOS pour Mac et configurer ses propriétés.

Dans le code, répondre à l’Action du module de reconnaissance pour gérer le mouvement de déclenchement par l’utilisateur. Là encore, cela dans la même façon qu’il serait géré dans iOS.

#### <a name="discrete-gesture-states"></a>États de mouvement discrets

Pour les gestes discrètes, l’Action est appelée lorsque le mouvement est reconnu et un état (`WKGestureRecognizerState`) est affecté en tant que :

[![](quick-interaction-techniques-images/quick01.png "États de mouvement discrets")](quick-interaction-techniques-images/quick01.png#lightbox)

Tous les gestes discrètes démarre dans le `Possible` États et les transitions vers un le `Failed` ou `Recognized` état. Lorsque vous utilisez des gestes discrètes, le développeur ne traitent généralement directement avec l’état. Au lieu de cela, elles s’appuient sur l’Action appelée lorsque le mouvement est reconnu uniquement.

#### <a name="continuous-gesture-states"></a>États de mouvement continu

Mouvements continues sont légèrement différentes de gestes discret, où l’Action est appelée plusieurs fois, comme le mouvement est reconnu :

[![](quick-interaction-techniques-images/quick02.png "États de mouvement continu")](quick-interaction-techniques-images/quick02.png#lightbox)

Là encore, les mouvements continue commence dans le `Possible` état, mais ils la progression des mises à jour. Ici le développeur doit prendre en compte les état du module de reconnaissance et de mettre à jour de l’interface utilisateur de l’application pendant le `Changed` phase jusqu'à ce que le mouvement est enfin `Recognized` ou `Canceled`.

#### <a name="gesture-recognizer-usage-tips"></a>Conseils d’utilisation de module de reconnaissance de mouvement

Apple suggère ce qui suit lorsque vous travaillez avec des modules de reconnaissance de mouvement dans watchOS 3 :

- Ajoutez les modules de reconnaissance de mouvement pour regrouper des éléments au lieu des contrôles individuels. Dans la mesure où l’Apple Watch a une plus petite taille d’écran physiques, les éléments de groupe ont tendance à être plus volumineuses et plus facilités cibles pour l’utilisateur tape. En outre, les modules de reconnaissance de mouvement peuvent entrer en conflit avec intégrées mouvements déjà dans les contrôles d’interface utilisateur native.
- Définir des relations de dépendance dans la table de montage séquentiel de l’application watch.
- Certains mouvements sont prioritaires sur les autres types de mouvements, telles que :
    - Défilement
    - Forcer le tactile
 
### <a name="digital-crown-rotation"></a>Rotation de la couronne numérique

En implémentant la prise en charge de couronne numérique dans leurs applications watchOS 3, un développeur peut fournir une navigation accrue interactions de vitesse et précision pour leurs utilisateurs.

Depuis watchOS 2, l’application Apple Watch pourrait utiliser le `WKInterfacePicker` objet pour accéder à la couronne numérique en fournissant une liste de `WKPickerItems` et un Style de sélecteur (liste, empilées ou séquence d’images). watchOS autorisée ensuite l’utilisateur à utiliser la couronne numérique pour sélectionner un élément dans la liste.

Lorsque vous utilisez un `WKInterfacePicker`, WatchKit est gérant l’essentiel du travail par :

- Dessin de la liste et les éléments d’interface individuels.
- Traitement des événements couronne numérique.
- Appel d’une Action lorsqu’un élément est sélectionné.

Nouveau à watchOS 3, le développeur a maintenant un accès direct aux événements de rotation couronne numérique qui leur permet de créer leurs propres éléments d’interface utilisateur qui répondent aux valeurs de rotation.

Accès couronne numérique est assuré par les éléments suivants :

- `WKCrownSequencer` -Permet d’accéder à des rotations par seconde.
- `WKCrownDelegate` -Permet d’accéder aux événements de rotation delta.

#### <a name="rotations-per-second"></a>Rotations par seconde

L’accès à des Rotations par seconde à partir de la couronne numérique est utile lors de l’utilisation de physique en fonction des animations. Pour accéder à des Rotations par seconde, utilisez le `CrownSequencer` propriété de la `WKInterfaceController` de l’Extension Watch. Exemple :

```csharp
var rotationsPerSecond = CrownSequencer.RotationsPerSecond;
```

#### <a name="rotational-deltas"></a>Rotation Deltas

Utilisez les Deltas de rotation à partir de la couronne numérique pour compter le nombre de rotations. Utilisez le `CrownDidRotate` substituer la méthode de la `WKCrownDelegate` pour accéder aux Deltas de rotation. Exemple :

```csharp
using System;
using WatchKit;
using Foundation;

namespace MonkeyWatch.MonkeySeeExtension
{
    public class CrownDelegate : WKCrownDelegate
    {
        #region Computed Properties
        public double AccumulatedRotations { get; set;}
        #endregion

        #region Constructors
        public CrownDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void CrownDidRotate (WKCrownSequencer crownSequencer, double rotationalDelta)
        {
            base.CrownDidRotate (crownSequencer, rotationalDelta);

            // Accumulate rotations
            AccumulatedRotations += rotationalDelta;
        }
        #endregion
    }
}
```

Ici l’application conserve une accumulation (`AccumulatedRotations`) pour déterminer le nombre de rotations. Une rotation complète de la couronne numérique est égale à un delta accumulé de `1.0` et une rotation de moitié serait `0.5`.

Apple a laissé au développeur de déterminer la manière dont les nombres de rotation correspondent à la sensibilité des modifications sur l’élément d’interface utilisateur mis à jour.

Le signe (`+/-`) de la rotation Delta indique la direction que l’utilisateur qui tente d’activer la couronne numérique :

[![](quick-interaction-techniques-images/quick03.png "Le signe de la rotation Delta indique la direction que l’utilisateur qui tente d’activer la couronne numérique")](quick-interaction-techniques-images/quick03.png#lightbox)


Si l’utilisateur fait défiler les, WatchKit retournera les deltas positif et si le défilement vers le bas, puis les deltas négatif seront renvoyés, peu importe quelle orientation de l’utilisateur a porter l’observation dans.

#### <a name="digital-crown-focus"></a>Couronne numérique Focus

Tout comme d’autres éléments d’interface, la couronne numérique propose le concept de Focus. Ce Focus peut être déplacé en dehors de la couronne numérique à d’autres éléments d’interface basés sur la façon dont l’utilisateur interagit avec la montre. 

Par exemple, les contrôles suivants peuvent voler le Focus de la couronne numérique :

- Sélecteur
- Curseur
- Contrôleur de défilement

Il incombe au développeur de déterminer le moment de leur élément d’interface personnalisée doit être le Focus de la couronne numérique. Apple suggère à l’aide des nouveaux modules de reconnaissance de mouvement pour obtenir le focus dans l’élément d’interface utilisateur personnalisée.

### <a name="vertical-paging"></a>Pagination verticale

Le standard qu’un utilisateur accède à une vue de Table dans une application watchOS consiste à faire défiler à l’élément souhaité de données, appuyez sur une ligne spécifique pour afficher la vue détaillée, appuyez sur le bouton précédent lorsque fini de consulter les détails et répétez le processus pour toutes les autres informations qui le y intéresse à partir de la table :

[![](quick-interaction-techniques-images/quick04.png "Déplacement entre une table et l’affichage des détails")](quick-interaction-techniques-images/quick04.png#lightbox)

Nouveau à watchOS 3, le développeur peut activer pagination verticale sur leurs contrôles d’affichage de la Table. Avec cette fonctionnalité activée, l’utilisateur peut faire défiler pour trouver une ligne de la vue de Table, puis appuyez sur la ligne pour afficher ses détails comme avant. Toutefois, ils peuvent désormais balayer pour sélectionner la ligne suivante dans la table ou pour sélectionner la ligne précédente (ou utilisez la couronne numérique), tout cela sans avoir à revenir à la vue Table tout d’abord :

[![](quick-interaction-techniques-images/quick05.png "Déplacement entre une table et l’affichage des détails et le glissement haut et bas pour vous déplacer entre les autres lignes")](quick-interaction-techniques-images/quick05.png#lightbox)

Pour activer ce mode, ouvrez Storyboard l’application watchOS dans Xcode pour la modification, sélectionnez la vue de Table et vérifiez le **Vertical en détail la pagination** case à cocher :

[![](quick-interaction-techniques-images/quick06.png "Cochez la case de pagination de détail verticale")](quick-interaction-techniques-images/quick06.png#lightbox)

Assurez-vous que la Table est à l’aide de Segues pour afficher la vue détaillée et enregistrez les modifications dans la table de montage séquentiel et revenir à Visual Studio pour Mac pour la synchronisation.

Le développeur peut par programmation s’engager pagination verticale à une ligne spécifique en utilisant le code suivant sur une vue de Table :

```csharp
// Segue into Vertical Paging and select the first row
MenuTable.PerformSegue (0);
```

Lorsque vous utilisez une pagination verticale, le développeur doit être conscient que WatchKit gère automatiquement le préchargement de contrôleurs et par conséquent, certaines méthodes de cycle de vie du contrôleur peuvent être appelées avant que l’interface utilisateur est réellement visible.

### <a name="notification-enhancements"></a>Améliorations de la notification

Notification constituent la principale d’Interaction rapide qui se traduisent généralement sur watchOS et ont été disponible depuis la première Apple Watch et watchOS 1.

Une Interaction rapide Notification typique est la suivante :

1. L’utilisateur sent le HAPTIQUE Notification lorsqu’une nouvelle Notification est reçue.
2. Elles déclenchent leur poignet pour afficher l’interface rechercher court pour la Notification.
3. Si elles continuent à conserver leur poignet déclenché, watchOS passe automatiquement à l’interface de Notification de regarder Long.

Il existe plusieurs façons un utilisateur peut répondre à la Notification :

- Pour r Hé bien définis et présentées de Notification, l’utilisateur ne rien faire et ignorez simplement la Notification.
- Ils peuvent aussi appuyer sur la notification pour lancer l’application watchOS.
- Pour une Notification qui prend en charge des Actions personnalisées, l’utilisateur peut sélectionner une des actions personnalisées. Il peut s’agir soit :
    - **Actions de premier plan** -ces lancent l’application pour effectuer l’action.
    - **Actions d’arrière-plan** - toujours routées vers l’iPhone dans watchOS 2, mais peuvent être routées vers le watchApp dans watchOS 3.

Nouveauté pour watchOS 3 :

* Notification utiliser une API similaire sur toutes les plateformes (iOS, watchOS, tvOS et macOS).
* Notification locale peut être planifiée sur l’Apple Watch.
* Notification d’arrière-plan sera routée à Extension de l’application si elles ont été planifiées sur l’Apple Watch.

#### <a name="notification-scheduling-and-delivery"></a>Planification de la notification et de remise

Notification à partir de l’iPhone de l’utilisateur sera vers l’avant à l’Apple Watch cas suivants :

* Écran de l’iPhone est désactivée.
* L’Apple Watch est porté et a été déverrouillée.

Dans watchOS 3, les Notifications locales peuvent être planifiées sur l’Apple Watch et soient uniquement remises sur la surveillance. Il incombe au développeur de planifier un iPhone correspondant Notification Si elle est requise par l’application.

En incluant le même identificateur de Notification sur les versions d’iPhone des Notifications et Apple Watch, il empêche les Notifications en double s’affiche sur la surveillance. La version de l’Apple Watch de la Notification est prioritaire par rapport à la version iPhone.

Étant donné que watchOS 3 utilise les mêmes `UINotification` infrastructure d’API comme iOS 10, consultez notre iOS 10 [infrastructure de Notification utilisateur](~/ios/platform/user-notifications/index.md) documentation pour plus d’informations.

### <a name="using-spritekit-and-scenekit"></a>À l’aide de SpriteKit et SceneKit

Nouveau à watchOS 3, le développeur peut désormais utiliser des objets SpritKit et SceneKit dans la conception de l’Interface utilisateur de leur application pour présenter les graphiques 2D et 3D.

Deux nouvelles classes d’interface ont été ajoutées pour prendre en charge cette fonctionnalité :

- `WKInterfaceSKScene` -Pour travailler avec des graphiques 2D SpriteKit.
- `WKInterfaceSCNScene` -Pour une utilisation avec des graphiques 3D SceneKit.

Pour utiliser ces objets, faites-les glisser jusqu'à l’aire de conception à l’intérieur d’animation de l’application watch dans Interface Builder de Xcode et utiliser le **inspecteur d’attributs** leur configuration.

À ce stade, utilisation de scènes SceneKit ou SpriteKit fonctionne comme elle le fait à l’intérieur d’une application iOS. L’application watch présentera un `WKInterfaceSKScene` en appelant une de le `Present` méthodes. Pour SceneKit, il suffit de définir la `Scene` propriété de la `WKInterfaceSCNScene` objet.

## <a name="actionable-complications"></a>Complications actionnables

Dans watchOS 2, Apple a introduit des Complications pour les applications tierces 3e. Dans watchOS 3, Apple a étendu les capacités un développeur peut inclure dans une Complication WatchKit. 

En outre, plusieurs intégrée dans Espion visages peuvent désormais inclure des Complications et Espion existante est confrontée que déjà prises en charge de Complications peuvent désormais inclus encore plus de Complications.

Également nouveau est la possibilité pour un utilisateur rapidement balayez gauche ou droite pour passer par tous les visages espion que dont ils disposent sur leur Apple Watch. À l’aide de la nouvelle galerie sur l’application iPhone de Compagnon de l’Apple Watch, l’utilisateur peut ajouter et personnaliser les nouvelles faces espion et une des Complications qu’ils peuvent inclure.

En raison de ces nouvelles fonctionnalités, Apple suggère que toutes les applications Apple Watch doivent également inclure au moins une Complication et par conséquent, toutes de l’application Native Apple Watch présentent désormais des Complications.

Complications fournissent les fonctionnalités suivantes à une application :

- Ils sont hautement glanceable dans la mesure où ils sont toujours présents sur le cadran de montre.
- Complications sont fréquemment mis à jour par watchOS. N’importe quelle application qui inclut une Complication sur l’utilisateur actuellement affiché cadran de montre est mis à jour deux fois au moins une heure.
- N’importe quelle application avec une Complication sur l’utilisateur actuellement affiché cadran de montre est conservée en mémoire, ce qui rend l’application à lancer rapidement et améliore la vitesse des réponses à partir de l’application.
- Complications facilitent le lancement des fonctionnalités spécifiques dans une application watchOS.

## <a name="glanceable-notification"></a>Notification glanceable

Notification sur Apple Watch permettent efficaces et personnalisables pour informer rapidement l’utilisateur d’événements ou de nouvelles informations telles que les messages entrants ou d’atteindre un objectif dans une application d’entraînement.

En utilisant une Notification, des informations précieuses peuvent rapidement être présentées à l’utilisateur. Dans de nombreuses situations, une Notification bien conçue peut éliminer la nécessité pour l’utilisateur à se lancer véritablement l’application.

Nouveau à watchOS 3, prise en charge de toutes les notifications maintenant :

- SpriteKit
- SceneKit
- Vidéo de inline

## <a name="enhanced-ui-with-spritekit-and-scenekit"></a>Interface utilisateur améliorée avec SpriteKit et SceneKit

En règle générale, un développeur peut se l’interface utilisateur de jeu SpriteKit et SceneKit sont mentionnés. Toutefois, les SpriteKit et SceneKit peuvent être utile pour la création d’interfaces utilisateur qui incluent des dispositions personnalisées, le contenu et les animations qui ne sont pas possibles dans WatchKit autonome sans jeu.

Par exemple, une notification de l’utilisateur à partir d’une application de partage de photos peut utiliser SpriteKit pour fournir une expérience utilisateur riche en incluant l’utilisateur qui a publié la photo, ainsi que d’une image réelle et d’autres informations personnalisées qui enrichit l’expérience utilisateur.

En outre, SpriteKit et SceneKit peuvent être combiné avec des éléments WatchKit UI standard dans la conception d’Interface utilisateur de l’application.

## <a name="simple-navigation"></a>Navigation simple

watchOS 3 présente plusieurs façons qu’un développeur peut simplifier la navigation au sein de leurs applications watchOS telles que le nouveau [pagination Vertical](#vertical-paging), [prise en charge du module de reconnaissance de mouvement](#gesture-recognizer-support) et [couronne numérique Rotation](#digital-crown-rotation) fonctionnalités présentées ci-dessus.

Le Crown numérique est unique à l’Apple Watch et peut être utilisé de différentes manières pour simplifier la navigation. Par exemple, une application de minuteur permettent la couronne numérique lire à vitesse variable par le biais des longueurs de minuteur disponibles.

Des mouvements personnalisés peuvent présenter des moyens nouveaux et uniques pour l’utilisateur d’interagir avec une application watch et peuvent également être utilisées pour simplifier la navigation de l’application.

Apple suggère recherchent des moyens de combiner toutes les nouvelles fonctionnalités d’Interaction rapide ajoutées dans watchOS 3 pour présenter les riches, facile et rapide d’utiliser des interfaces d’application watchOS.

## <a name="finishing-the-quick-interaction"></a>Terminer l’Interaction rapide

Une expérience d’interaction rapide bien conçue permet aux utilisateurs la confiance nécessaire pour supprimer leur poignet (et désengager avec l’application) quand ceux-ci ont terminé l’interaction en cours.

Où cela devient plus précisément un problème est lorsque l’application watch est n’importe quel type de connexion réseau ou de partage des informations avec son application iPhone de complément. Ceci peut souvent mener à un indicateur d’attente, tandis que la transaction est en cours, ce qui n’est pas souhaitable pendant une interaction rapide. Prenons l’exemple suivant :

[![](quick-interaction-techniques-images/quick07.png "Diagramme de l’application watch établit une connexion réseau et partage des informations avec son application iPhone de complément")](quick-interaction-techniques-images/quick07.png#lightbox)

1. L’utilisateur choisit un élément pour effectuer des achats dans l’espion.
2. Ils appuyer sur le bouton acheter.
3. L’application démarre la transaction réseau et affiche un indicateur de chargement.
4. Quelques instants plus tard, la transaction se termine et l’application affiche une confirmation d’achat.
5. L’utilisateur supprime leur poignet et permet de dégager avec l’application.

À partir du moment que l’utilisateur appuie sur le bouton acheter jusqu'à ce que la transaction est terminée, ils ont leur poignet déclenché en examinant un indicateur de chargement. Pour résoudre cette situation, Apple suggère que le développeur doit présenter des commentaires instantanés à l’utilisateur au lieu d’afficher un indicateur de chargement. 

À l’aide du modèle suggéré d’Apple, examinons l’interaction rapide même à nouveau :

[![](quick-interaction-techniques-images/quick08.png "Diagramme de modèle suggéré de pommes")](quick-interaction-techniques-images/quick08.png#lightbox)

1. L’utilisateur choisit un élément pour effectuer des achats dans l’espion.
2. Ils appuyer sur le bouton acheter.
3. L’application démarre la transaction réseau et affiche un message indiquant que l’achat a démarré avec succès.
4. L’utilisateur supprime leur poignet et permet de dégager avec l’application.
5. Lorsque la transaction se termine correctement quelques instants plus tard, l’application affiche une Notification locale pour informer l’utilisateur d’un achat réussi.

Cette fois, dès que l’utilisateur appuie sur le bouton acheter que s’affiche un message indiquant que l’achat a démarré, afin de pouvoir en toute confiance supprimer leur poignet et mettre fin à l’interaction rapide à ce stade. Plus tard, ils sont informés de la réussite ou l’échec de la transaction dans une Notification à l’utilisateur. De cette façon, l’utilisateur interagit uniquement avec l’application au cours des phases du processus « actifs ».

Pour les applications qui font de mise en réseau, ils peuvent utiliser un arrière-plan `NSURLSession` pour gérer la communication réseau avec une tâche de téléchargement. Cela permettra à l’application d’être sorti en arrière-plan pour traiter les informations téléchargées. Pour les applications nécessitant un traitement en arrière-plan, utilisez une Assertion de la tâche en arrière-plan pour gérer le traitement requis.

## <a name="quick-interaction-design-tips"></a>Conseils de conception d’Interaction rapide

Dans la mesure où la longueur souhaitée d’une Interaction rapide est de deux secondes ou moins, le développeur doit se concentrer sur la conception d’interactions de l’application depuis le début du processus de conception. Recherchez les zones où ces interactions peuvent être simplifiées (à l’aide de la technique présentée ci-dessus) et utilisent les nouvelles fonctionnalités de watchOS 3 pour rendre l’application rapide et réactif.

Apple suggère les éléments suivants :

- Pencher sur les Interactions rapide en intégrant les fonctionnalités les plus utilisées de l’application vers l’avant.
- Utiliser des Complications et des Notifications à l’utilisateur à la surface des fonctions et fonctionnalités courantes.
- Créez glanceable riche Interface utilisateur avec SceneKit et SpriteKit.
- Si possible, simplifient la navigation au sein de l’application.
- Ne jamais définir l’utilisateur attendez, de les autoriser à supprimer leur poignet et désengager dès que possible avec l’application.

## <a name="summary"></a>Récapitulatif

Cet article a présenté les techniques d’interaction rapide Apple a ajouté dans watchOS 3 et comment les implémenter dans Xamarin.iOS pour Apple Watch.

## <a name="related-links"></a>Liens associés

- [Exemples watchOS](https://developer.xamarin.com/samples/watchos/all/)
