---
title: "Techniques d’Interaction rapide pour watchOS 3"
description: "Cet article décrit les techniques d’interaction rapide Apple a ajouté dans watchOS 3 et comment les implémenter dans Xamarin.iOS pour Apple Watch."
ms.topic: article
ms.prod: xamarin
ms.assetid: 26697F68-AF7E-4A36-988F-85E2674A4DD1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 75a8e807a68a3fccfa76fc7ba1f260818b25174d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="quick-interaction-techniques-for-watchos-3"></a>Techniques d’Interaction rapide pour watchOS 3

_Cet article décrit les techniques d’interaction rapide Apple a ajouté dans watchOS 3 et comment les implémenter dans Xamarin.iOS pour Apple Watch._

Les interactions utilisateur rapide en fournissant sont essentielles à la création d’applications Apple Watch exceptionnelles et les Complications. Nouveau watchOS 3, Apple a ajouté la prise en charge pour les modules de reconnaissance de mouvement, accéder aux nouvelles techniques de Notification utilisateur et la navigation et couronne numérique. Cette opération, ainsi que la prise en charge ajoutée pour SceneKit et SpriteKit, permettent au développeur de créer facilement des interfaces riches, parcourez-les rapides et réactives.

## <a name="what-are-quick-interactions"></a>Quelles sont les Interactions rapide

Pour un développeur qui est utilisé pour la création d’applications pour iOS ou macOS (la quantité de temps pendant lequel l’utilisateur interagit avec l’application est exprimée en minutes ou en heures), vous concevez une application réussie de l’Apple Watch peut être difficile et nécessite une autre approche.

Dans watchOS, l’utilisateur souhaite généralement déclencher leur poignet, rapidement interagir avec une application (généralement pour brève quelques secondes), puis supprimer leur poignet et continuer qui était que qu’ils ont été faites.

Voici quelques exemples d’interactions rapides standard sur l’Apple Watch :

- Démarre un minuteur.
- Vérification de la météo.
- Marquage d’un élément hors d’une liste de tâches.

Pour atteindre ces objectifs, une application sur l’Apple Watch doit être :

- **Parcourez-les** -ce qui signifie que, avec une rapide aperçu de l’utilisateur doit être en mesure d’obtenir les informations dont ils ont besoin. 
- **Utilisables** -ce qui signifie que les utilisateurs doit-elle être en mesure de prendre des décisions éclairées rapides.
- **Réactive** -ce qui signifie que l’utilisateur ne doit jamais attendre de recevoir les informations dont ils ont besoin ou pour obtenir de l’action qu’il souhaite.

### <a name="quick-interactions-length"></a>Longueur des Interactions rapide

En raison de la nature parcourez-les des applications de l’Apple Watch, Apple suggère que la longueur idéale d’une interaction rapide doit être de deux secondes ou moins. À la suite de cette deuxième deux limite, le développeur doit passent beaucoup de temps à la fois la conception et implémentation d’une application Apple Watch. 

## <a name="new-watchos-3-features-and-apis"></a>Nouvelle watchOS 3 fonctionnalités et API

Apple a ajouté de nouvelles fonctionnalités et API à WatchKit pour aider le développeur de l’ajout des interactions rapides à leurs applications Apple Watch :

- watchOS 3 fournit l’accès aux nouveaux types d’entrée comme utilisateur :
    - Modules de reconnaissance de mouvement
    - Rotation de couronne numérique 
- watchOS 3 fournit de nouvelles façons d’afficher et de mise à jour des informations, telles que :
    - Navigation améliorée dans les tables
    - Nouvelle prise en charge de framework Notification utilisateur
    - Intégration de SpriteKit et SceneKit

En implémentant ces nouvelles fonctionnalités, le développeur peut s’assurer que leur application watchOS 3 est Glanceable, exploitable et réactivité.

### <a name="gesture-recognizer-support"></a>Prise en charge du module de reconnaissance de mouvement

Si le développeur a implémenté les reconnaissances de mouvement dans iOS, ils doivent être très bien connaître les modules de reconnaissance de mouvement dans watchOS 3. Pour actualiser, reconnaissances de mouvement sont des objets qui analysent les événements tactiles de bas niveau en mouvements reconnaissables, prédéfinis.

watchOS 3 prend en charge les quatre reconnaissances de mouvement suivantes :

- Types de mouvements discrètes :
    - Le mouvement de balayage (`WKSwipeGestureRecognizer`).
    - L’action d’appuyer (`WKTapGestureRecognizer`).
- Types de mouvement continue :
    - Le mouvement panoramique (`WKPanGestureRecognizer`).
    - Le mouvement de longue durée, appuyez sur (`WKLongPressGestureRecognizer`).

Pour implémenter l’une de la nouvelle reconnaissances de mouvement, simplement faire glisser sur une aire de conception dans le concepteur dans Visual Studio d’iOS pour Mac et configurer ses propriétés.

Dans le code, répondre à l’Action de la reconnaissance à gérer le mouvement déclenché par l’utilisateur. Là encore, cela de la même manière qu’elle est gérée dans iOS.

#### <a name="discrete-gesture-states"></a>États de mouvement discrets

Pour les mouvements discrètes, l’Action est appelée lorsque le mouvement est reconnu et un état (`WKGestureRecognizerState`) est attribué en tant que :

[ ![](quick-interaction-techniques-images/quick01.png "États de mouvement discrets")](quick-interaction-techniques-images/quick01.png)

Tous les mouvements discrètes démarrent le `Possible` état et la transition vers un le `Failed` ou `Recognized` état. Lorsque vous utilisez des mouvements discrètes, le développeur ne traitent généralement directement avec l’état. Au lieu de cela, elles s’appuient sur l’Action qui est appelée lorsque le mouvement est reconnu uniquement.

#### <a name="continuous-gesture-states"></a>États de mouvement continu

Les mouvements continues diffèrent légèrement de mouvements discret, où l’Action est appelée plusieurs fois, comme les mouvements est reconnu :

[ ![](quick-interaction-techniques-images/quick02.png "États de mouvement continu")](quick-interaction-techniques-images/quick02.png)

Là encore, les mouvements continue commence dans le `Possible` état, mais ils la progression des mises à jour. Ici le développeur doit prendre en compte les état du module de reconnaissance et de mettre à jour de l’interface utilisateur de l’application pendant la `Changed` phase jusqu'à ce que le mouvement est enfin `Recognized` ou `Canceled`.

#### <a name="gesture-recognizer-usage-tips"></a>Conseils d’utilisation de module de reconnaissance de mouvement

Apple suggérer les éléments suivants lorsque vous travaillez avec des modules de reconnaissance de mouvement dans watchOS 3 :

- Ajoutez les modules de reconnaissance de mouvement pour regrouper des éléments à la place des contrôles individuels. Étant donné que l’Apple Watch a une plus petite taille d’écran physiques, les éléments de groupe ont tendance à être plus grande et plus faciles cibles pour l’utilisateur. En outre, les modules de reconnaissance de mouvement peuvent entrer en conflit avec intégré mouvements déjà dans les contrôles d’interface utilisateur native.
- Définir des relations de dépendance dans la table de montage séquentiel de l’application de surveillance.
- Certains mouvements sont prioritaires sur les autres types de mouvement, telles que :
    - Le défilement
    - Force tactile
 
### <a name="digital-crown-rotation"></a>Rotation de couronne numérique

En implémentant une prise en charge couronne numériques dans leurs applications watchOS 3, un développeur peut fournir une navigation accrue interactions de vitesse et de précision pour les utilisateurs.

Depuis watchOS 2, Apple Watch de l’application peut utiliser le `WKInterfacePicker` objet pour accéder à la couronne numérique en fournissant une liste de `WKPickerItems` et un Style de sélecteur (liste, empilées ou séquence d’images). watchOS autorisée ensuite l’utilisateur à utiliser la couronne numérique pour sélectionner un élément dans la liste.

Lorsque vous utilisez un `WKInterfacePicker`, WatchKit gère la plupart du travail par :

- Dessin de la liste et les éléments d’interface individuels.
- Traitement des événements couronne numérique.
- Appel d’une Action lorsqu’un élément est sélectionné.

Nouveau pour watchOS 3, le développeur a maintenant un accès direct aux événements rotation couronne numérique qui leur permet de créer leurs propres éléments d’interface utilisateur qui répondent à des valeurs de rotation.

Accès couronne numérique est fourni par les éléments suivants :

- `WKCrownSequencer` -Permet d’accéder à des rotations par seconde.
- `WKCrownDelegate` -Permet d’accéder à des événements de rotation delta.

#### <a name="rotations-per-second"></a>Rotations par seconde

L’accès à des Rotations par seconde à partir de la couronne numérique est utile lors de l’utilisation de physique en fonction des animations. Pour accéder à des Rotations par seconde, utilisez le `CrownSequencer` propriété de la `WKInterfaceController` de l’Extension de surveillance. Exemple :

```csharp
var rotationsPerSecond = CrownSequencer.RotationsPerSecond;
```

#### <a name="rotational-deltas"></a>Rotation Deltas

Les Deltas de rotation à partir de la couronne numérique permet de compter le nombre de rotations. Utilisez le `CrownDidRotate` substituer la méthode de le `WKCrownDelegate` pour accéder aux Deltas de rotation. Exemple :

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

Ici l’application conserve une accumulation (`AccumulatedRotations`) pour déterminer le nombre de rotations. Une rotation complète de la couronne numérique est égale à un delta cumulé de `1.0` et une rotation moitié serait `0.5`.

Apple a laissé au développeur de déterminer la façon dont les nombres de rotation correspondent à la sensibilité des modifications sur l’élément d’interface utilisateur en cours de mise à jour.

Le signe (`+/-`) de la rotation Delta indique la direction que l’utilisateur aura la couronne numérique :

[ ![](quick-interaction-techniques-images/quick03.png "Le signe de l’écart de rotation indique la direction que l’utilisateur aura la couronne numérique")](quick-interaction-techniques-images/quick03.png)


Si l’utilisateur est défilement vers le haut, WatchKit retournera les deltas positifs et si le défilement vers le bas, puis deltas négatifs seront renvoyés, quel que soit le quelles orientation de l’utilisateur porte l’observation dans.

#### <a name="digital-crown-focus"></a>Focus de couronne numérique

Tout comme les autres éléments d’interface, la couronne numérique propose le concept de Focus. Cet Focus peut être déplacé en dehors de la couronne numérique à d’autres éléments d’interface en fonction de la façon dont l’utilisateur interagit avec la montre. 

Par exemple, un des contrôles suivants risquent d’usurper le Focus de la couronne numérique :

- Sélecteur
- Curseur
- Contrôleur de défilement

Il incombe au développeur de déterminer quand l’élément d’interface personnalisée doit être le Focus de la couronne numérique. Apple suggère à l’aide de la nouvelle reconnaissances de mouvement pour obtenir le focus dans l’élément d’interface utilisateur personnalisée.

### <a name="vertical-paging"></a>Pagination verticale

Le standard qu’un utilisateur accède à une vue de Table dans une application watchOS consiste à accéder à l’élément souhaité de données, appuyer sur une ligne spécifique pour afficher la vue détaillée, cliquez sur le bouton précédent lorsque terminé de consulter les détails et répétez le processus pour toute autre information qui le y intéressent à partir de la table :

[ ![](quick-interaction-techniques-images/quick04.png "Déplacement entre une table et l’affichage des détails")](quick-interaction-techniques-images/quick04.png)

Nouveau pour watchOS 3, le développeur peut activer la pagination verticale sur leurs contrôles d’affichage de la Table. Avec cette fonctionnalité est activée, l’utilisateur peut faire défiler pour rechercher une ligne de la Table de vue, puis appuyez sur la ligne pour afficher ses détails comme avant. Toutefois, ils peuvent maintenant balayez vers haut pour sélectionner la ligne suivante dans la table, ou pour sélectionner la ligne précédente (ou utilisez la couronne numérique), tout cela sans avoir à revenir à l’affichage de la Table tout d’abord :

[ ![](quick-interaction-techniques-images/quick05.png "Déplacement entre une table et la vue de détail et le glissement haut et bas pour vous déplacer entre les autres lignes")](quick-interaction-techniques-images/quick05.png)

Pour activer ce mode, ouvrez Storyboard l’application watchOS dans Xcode pour la modification, sélectionnez la vue de Table et vérifier les **Vertical en détail la pagination** case à cocher :

[ ![](quick-interaction-techniques-images/quick06.png "Cochez la case de pagination de détail Vertical")](quick-interaction-techniques-images/quick06.png)

Assurez-vous que la Table utilise Segues pour afficher la vue détaillée et enregistrez les modifications dans la table de montage séquentiel et retournez dans Visual Studio pour Mac à synchroniser.

Le développeur peut par programmation prendre part aux pagination Vertical pour une ligne spécifique dans le code suivant sur une vue de Table :

```csharp
// Segue into Vertical Paging and select the first row
MenuTable.PerformSegue (0);
```

Lorsque vous utilisez une pagination verticale, le développeur doit savoir que WatchKit gère automatiquement le préchargement de contrôleurs et par conséquent, certaines méthodes de cycle de vie du contrôleur peuvent être appelées avant que l’interface utilisateur est en fait visible.

### <a name="notification-enhancements"></a>Améliorations de la notification

Notification sont principalement la forme d’Interaction rapide qu’un utilisateur consomme généralement sur watchOS et ont été disponible depuis le premier Apple Watch et watchOS 1.

Une Interaction rapide Notification typique est la suivante :

1. L’utilisateur estime la Notification tactile lorsqu’une Notification est reçue.
2. Ils déclenchent leur poignet pour afficher l’interface rechercher courte pour la Notification.
3. Si elles continuent à conserver leur poignet déclenché, watchOS passe automatiquement à l’interface de Notification de regarder Long.

Il existe plusieurs façons dont un utilisateur peut répondre à la Notification :

- Pour bien définis et présentés de Notification, l’utilisateur ne fait rien et simplement faire disparaître la Notification.
- Ils peuvent aussi appuyer sur la notification pour lancer l’application watchOS.
- Pour une Notification qui prend en charge des Actions personnalisées, l’utilisateur peut sélectionner une des actions personnalisées. Il peut s’agir soit :
    - **Actions de premier plan** -ils lancent l’application pour effectuer l’action.
    - **Actions d’arrière-plan** - routées toujours pour l’iPhone dans watchOS 2, mais peuvent être acheminés vers le watchApp dans watchOS 3.

Pour watchOS 3 :

* Notification d’utiliser une API similaire sur toutes les plateformes (iOS, watchOS, tvOS et macOS).
* Notification locale peut être planifiée sur l’Apple Watch.
* Notification en arrière-plan est routée vers l’Extension de l’application si elles ont été planifiées sur l’Apple Watch.

#### <a name="notification-scheduling-and-delivery"></a>Planification de la notification et de remise

Les notifications à partir de l’iPhone de l’utilisateur sera vers l’avant à l’Apple Watch cas suivants :

* Écran de l’iPhone est désactivée.
* L’Apple Watch est porté et a été déverrouillée.

Dans watchOS 3, Notifications Local peuvent être planifiées sur l’Apple Watch et sont remises uniquement sur la surveillance. C’est au développeur de planifier un iPhone correspondant Notification Si elle est requise par l’application.

En incluant le même identificateur de Notification sur les versions d’iPhone des Notifications et Apple Watch, il empêche les Notifications en double s’affiche sur la surveillance. La version de l’Apple Watch de la Notification est prioritaire sur la version de l’iPhone.

Étant donné que watchOS 3 utilise la même `UINotification` framework API iOS 10, consultez notre iOS 10 [Framework de Notification utilisateur](~/ios/platform/user-notifications/index.md) documentation pour plus de détails.

### <a name="using-spritekit-and-scenekit"></a>À l’aide de SpriteKit et SceneKit

Nouveau pour watchOS 3, le développeur peut maintenant utiliser des objets SpritKit et SceneKit dans la conception de l’Interface utilisateur de leur application pour présenter les graphiques 2D et 3D.

Deux nouvelles classes d’interface ont été ajoutés pour prendre en charge cette fonctionnalité :

- `WKInterfaceSKScene` -Pour travailler avec les graphiques 2D SpriteKit.
- `WKInterfaceSCNScene` -Pour travailler avec des graphiques 3D SceneKit.

Pour utiliser ces objets, faites-les glisser vers l’aire de conception à l’intérieur du plan conceptuel de l’application de la surveillance dans le Générateur de Xcode Interface et utiliser le **attributs inspecteur** pour les configurer.

À ce stade, utilisation des scènes SceneKit ou SpriteKit fonctionne comme il le fait à l’intérieur d’une application iOS. L’application watch présentera un `WKInterfaceSKScene` en appelant une de le `Present` méthodes. Pour SceneKit, définissez simplement le `Scene` propriété de la `WKInterfaceSCNScene` objet.

## <a name="actionable-complications"></a>Complications utilisables

Dans watchOS 2, Apple a introduit le Complications pour les applications tierces 3e. Dans watchOS 3, Apple a étendu les capacités un développeur peut inclure dans le des complications WatchKit. 

En outre, plusieurs intégré dans Espion faces peuvent maintenant inclure des Complications et Espion existante fait face que déjà Complications pris en charge peuvent désormais incluse Complications davantage.

Également nouveau est la possibilité pour un utilisateur à rapidement balayez gauche ou droite pour passer à travers tous les faces espion installé sur leur Apple Watch. À l’aide de la galerie de nouveau sur l’application d’iPhone d’accompagnement de l’Apple Watch, l’utilisateur peut ajouter et personnaliser des faces espion nouvelle et une des Complications qu’ils peuvent inclure.

En raison de ces nouvelles fonctionnalités, Apple suggère que chaque application sur Apple Watch doit également inclure au moins une des complications et par conséquent, tous les l’application Native Apple Watch ont maintenant des Complications.

Complications fournissent les fonctionnalités suivantes à une application :

- Ils sont hautement parcourez-les, car ils sont toujours présents sur le cadran de la montre.
- Complications sont fréquemment mis à jour par watchOS. N’importe quelle application qui inclut le des complications sur face espion actuellement affiché de l’utilisateur est mis à jour deux fois au moins une heure.
- N’importe quelle application avec le des complications sur face espion actuellement affiché de l’utilisateur est conservée en mémoire, ce qui rend l’application à lancer rapidement et améliore la vitesse des réponses à partir de l’application.
- Complications facilitent le lancement des fonctionnalités spécifiques dans une application watchOS.

## <a name="glanceable-notification"></a>Notification parcourez-les

Notification sur Apple Watch permettent efficaces et personnalisable pour rapidement informer l’utilisateur d’événements ou de nouvelles informations telles que les messages entrants ou d’un objectif dans une application d’entraînement.

En utilisant une Notification, des informations importantes peuvent être rapidement présentées à l’utilisateur. Dans de nombreuses situations, une Notification bien conçue peut supprimer la nécessité pour l’utilisateur pour réellement lancer l’application.

Vous débutez avec watchOS 3, prise en charge de toutes les notifications maintenant :

- SpriteKit
- SceneKit
- Vidéo de inline

## <a name="enhanced-ui-with-spritekit-and-scenekit"></a>Interface utilisateur améliorée avec SpriteKit et SceneKit

En règle générale, un développeur peut considérer l’interface utilisateur de jeu lorsque SpriteKit et SceneKit sont mentionnés. Toutefois, les SpriteKit et SceneKit peuvent être utile pour la création d’interfaces utilisateur qui incluent des dispositions personnalisées, le contenu sans jeu et les animations ne sont pas possibles dans WatchKit uniquement dans le cas contraire.

Par exemple, une notification de l’utilisateur à partir d’une application de partage de photos peut utiliser SpriteKit pour fournir une expérience utilisateur riche en incluant l’utilisateur qui a publié l’image avec une image réelle et d’autres informations personnalisées qui enrichit l’expérience utilisateur.

En outre, SpriteKit et SceneKit peuvent être mélangés avec des éléments WatchKit UI standard dans la conception de l’Interface utilisateur de l’application.

## <a name="simple-navigation"></a>Navigation simple

watchOS 3 présente plusieurs façons qu’un développeur peut simplifier la navigation au sein de leurs applications watchOS telles que le nouveau [pagination Vertical](#Vertical-Paging), [prise en charge du module de reconnaissance de mouvement](#Gesture-Recognizer-Support) et [couronne numérique Rotation](#Digital-Crown-Rotation) fonctionnalités présentées ci-dessus.

La couronne numérique est unique à l’Apple Watch et peut être utilisée de différentes façons pour simplifier la navigation. Par exemple, une application du minuteur permet d’effectuer la couronne numérique lire à vitesse variable par le biais des longueurs de minuteur disponibles.

Des mouvements personnalisés peuvent présenter des façons nouvelles et uniques pour l’utilisateur d’interagir avec une application de surveillance et peuvent également être utilisés pour simplifier la navigation de l’application.

Apple suggérer cherchez à combiner toutes les nouvelles fonctionnalités d’Interaction rapide ajoutées dans watchOS 3 pour présenter enrichi, facile et rapide d’utiliser les interfaces d’application watchOS.

## <a name="finishing-the-quick-interaction"></a>Terminer l’Interaction rapide

Une expérience d’interaction rapide bien conçue permet aux utilisateurs la confiance à supprimer leur poignet (et dégager avec l’application) lorsqu’ils ont fini de l’interaction actuelle.

Plus précisément, cela devient un problème est lorsque l’application espion est n’importe quel type de connexion réseau ou de partager des informations avec son application d’iPhone d’accompagnement. Cela mène souvent à un indicateur en attente pendant que la transaction est en place, ce qui n’est pas souhaitable pendant une interaction rapide. Prenons l’exemple suivant :

[ ![](quick-interaction-techniques-images/quick07.png "Diagramme de l’application espion effectuant une connexion réseau et partage des informations avec son application d’iPhone d’accompagnement")](quick-interaction-techniques-images/quick07.png)

1. L’utilisateur choisit un élément d’achat sur la surveillance.
2. Ils appuyer sur le bouton acheter.
3. L’application démarre la transaction réseau et affiche un indicateur de chargement.
4. Quelques instants plus tard, la transaction se termine et l’application affiche une confirmation d’achat.
5. L’utilisateur supprime son poignet et déconnecte avec l’application.

À partir du moment que l’utilisateur appuie sur le bouton acheter jusqu'à ce que la transaction est terminée, elles ont leur poignet déclenché en examinant un indicateur de chargement. Pour résoudre cette situation, Apple suggère que le développeur doit présenter les commentaires instantanés à l’utilisateur au lieu d’afficher un indicateur de chargement. 

À l’aide du modèle suggéré de d’Apple, examinons l’interaction rapide même à nouveau :

[ ![](quick-interaction-techniques-images/quick08.png "Diagramme de modèle suggéré pommes")](quick-interaction-techniques-images/quick08.png)

1. L’utilisateur choisit un élément d’achat sur la surveillance.
2. Ils appuyer sur le bouton acheter.
3. L’application démarre la transaction réseau et affiche un message indiquant que le fournisseur a démarré.
4. L’utilisateur supprime son poignet et déconnecte avec l’application.
5. Lorsque la transaction terminée avec succès quelques instants plus tard, l’application affiche une Notification Local pour informer l’utilisateur d’un achat réussi.

Cette fois, dès que l’utilisateur appuie sur le bouton acheter, qu'elles s’affichent un message indiquant que le fournisseur a démarré, afin de supprimer leur poignet en toute confiance et mettre fin à l’interaction rapide à ce stade. Plus tard, ils sont informés de la réussite ou l’échec de la transaction dans une Notification à l’utilisateur. De cette façon, l’utilisateur est uniquement interagit avec l’application au cours des phases du processus de « actifs ».

Pour les applications qui effectuent la mise en réseau, ils peuvent utiliser un arrière-plan `NSURLSession` pour gérer la communication réseau avec une tâche de téléchargement. Ainsi, l’application d’être réveillés en arrière-plan pour traiter les informations téléchargées. Pour les applications qui nécessitent un traitement en arrière-plan, utilisez une Assertion de la tâche en arrière-plan pour gérer le traitement requis.

## <a name="quick-interaction-design-tips"></a>Conseils de création rapide

Étant donné que la longueur souhaitée d’une Interaction rapide est de deux secondes ou moins, le développeur doit se concentrer sur la conception d’interactions de l’application depuis le début du processus de conception. Identifier des zones où ces interactions peuvent être simplifiées (à l’aide de la technique présentée ci-dessus) et utilisent les nouvelles fonctionnalités de watchOS 3 pour rendre l’application rapide et réactives.

Apple suggère les éléments suivants :

- Vous concentrer sur les Interactions rapide en rassemblant les fonctionnalités les plus utilisées de l’application vers l’avant.
- Utilisez des Complications et des Notifications à l’utilisateur pour exposer des fonctions et fonctionnalités courantes.
- Créez parcourez-les riche Interface utilisateur avec SceneKit et SpriteKit.
- Chaque fois que possible, simplifier la navigation dans l’application.
- Ne modifiez jamais l’utilisateur attendre, et les autoriser à supprimer leur poignet et dégager avec l’application dès que possible.

## <a name="summary"></a>Récapitulatif

Cet article a couvert les techniques d’interaction rapide Apple a ajouté dans watchOS 3 et comment les implémenter dans Xamarin.iOS pour Apple Watch.

## <a name="related-links"></a>Liens associés

- [Exemples watchOS](https://developer.xamarin.com/samples/watchos/all/)
