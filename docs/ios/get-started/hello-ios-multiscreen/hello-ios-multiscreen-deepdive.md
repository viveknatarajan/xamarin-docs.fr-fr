---
title: Hello, iOS multi-écran - Immersion
description: Ce document examine de manière détaillée l’application Phoneword développée, en particulier model-view-controller, la navigation iOS et d’autres concepts du développement iOS.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: c866e5f4-8154-4342-876e-efa0693d66f5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/02/2016
ms.openlocfilehash: cdeea6d78ec1262a0b5b613b4f483012c9df2c19
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785656"
---
# <a name="hello-ios-multiscreen--deep-dive"></a>Hello, iOS multi-écran - Immersion

Au cours de la procédure pas à pas de démarrage rapide, nous avons généré et exécuté notre première application Xamarin.iOS multi-écran. Il est maintenant temps d’approfondir les notions de navigation et d’architecture iOS.

Dans ce guide, nous présentons le modèle *MVC (Model-View-Controller)* et son rôle dans l’architecture et la navigation iOS.
Ensuite, nous nous intéressons au contrôleur de navigation pour apprendre à l’utiliser afin d’offrir une expérience de navigation naturelle dans iOS.

<a name="Model_View_Controller" />

## <a name="model-view-controller-mvc"></a>MVC (Model-View-Controller)

Dans le didacticiel [Hello, iOS](~/ios/get-started/hello-ios/index.md), nous avons appris que les applications iOS ont une seule *fenêtre* et que les contrôleurs d’affichage sont responsables du chargement de leurs *hiérarchies d’affichage de contenu* dans cette fenêtre. Dans la deuxième procédure pas à pas Phoneword, nous avons ajouté un deuxième écran à notre application et passé certaines données (une liste de numéros de téléphone) entre les deux écrans, comme l’illustre le diagramme ci-dessous :

 [![](hello-ios-multiscreen-deepdive-images/08.png "Ce diagramme illustre le passage de données entre deux écrans")](hello-ios-multiscreen-deepdive-images/08.png#lightbox)

Dans notre exemple, les données étaient collectées dans le premier écran, passées du premier contrôleur d’affichage au deuxième, puis affichés par le deuxième écran. Cette séparation des écrans, contrôleurs d’affichage et données suit le modèle *, MVC (Model-View-Controller)*. Dans les sections suivantes, nous présentons les avantages de ce modèle, ses composants et la manière dont nous l’utilisons dans notre application Phoneword.

### <a name="benefits-of-the-mvc-pattern"></a>Avantages du modèle MVC

Le modèle MVC (Model-View-Controller) est un *modèle de conception*, autrement dit une solution d’architecture réutilisable à un problème courant ou cas d’usage dans le code. Le modèle MVC est une architecture destinée aux applications dotées d’une *interface graphique utilisateur (GUI)*. Il attribue aux objets dans l’application un des trois rôles suivants : *modèle* (Model) (données ou logique d’application), *affichage* (View) (interface utilisateur) et *contrôleur* (Controller) (code-behind). Le diagramme ci-dessous illustre les relations entre les trois éléments du modèle MVC et l’utilisateur :

 [![](hello-ios-multiscreen-deepdive-images/00.png "Ce diagramme illustre les relations entre les trois éléments du modèle MVC et l’utilisateur")](hello-ios-multiscreen-deepdive-images/00.png#lightbox)

Le modèle MVC s’avère utile car il fournit une séparation logique entre les différents composants d’une application GUI et facilite la réutilisation de code et d’affichages. Intéressons-nous maintenant de plus près à chacun des trois rôles.

> [!NOTE]
> Le modèle MVC est vaguement analogue à la structure des pages ASP.NET ou des applications WPF. Dans ces exemples, l’affichage (View) est le composant réellement chargé de décrire l’interface utilisateur ; il correspond à la page ASPX (HTML) dans ASP.NET ou au XAML dans une application WPF. Le contrôleur (Controller) est le composant chargé de gérer l’affichage (View), qui correspond au code-behind dans ASP.NET ou WPF.


### <a name="model"></a>Modèle

L’objet Model est généralement une représentation propre à l’application des données à afficher ou à entrer dans l’affichage (View). Il est souvent peu défini : par exemple, dans notre application **Phoneword_iOS**, la liste des numéros de téléphone (représentés sous la forme d’une liste de chaînes) est le modèle. Si nous voulions générer une application multiplateforme, nous pourrions choisir de partager le code **PhonewordTranslator** entre nos applications iOS et Android. Nous pourrions considérer ce code partagé comme le modèle également.

Le modèle MVC ne tient absolument pas compte de la *persistance des données* ni de l’*accès* de l’objet Model. En d’autres termes, le modèle MVC n’a que faire de ce à quoi ressemblent nos données ni de la manière dont elles sont stockées. Il s’intéresse uniquement à la manière dont elles sont *représentées*. Par exemple, nous pourrions choisir de stocker nos données dans une base de données SQL ou de les rendre persistantes dans un mécanisme de stockage cloud ou encore d’utiliser simplement `List<string>`. Pour MVC, seule la représentation des données est incluse dans le modèle.

Dans certains cas, la partie Model du modèle MVC peut être vide. Par exemple, nous pourrions choisir d’ajouter certaines pages statiques à notre application pour expliquer le fonctionnement du convertisseur de numéros de téléphone, les raisons de sa création et la manière de nous contacter pour signaler des bogues. Ces écrans d’application seraient quand même créés à l’aide d’affichages et de contrôleurs, mais ils n’auraient pas de données Model réelles.

> [!NOTE]
> Dans certains documents, la partie Modèle du modèle MVC peut faire référence au backend de toute l’application, pas seulement aux données qui s’affichent dans l’interface utilisateur. Dans ce guide, nous utilisons une interprétation moderne de la partie Model, mais cette distinction n’est pas particulièrement importante.


### <a name="view"></a>Vue

Un affichage (View) est un composant chargé du rendu de l’interface utilisateur. Dans presque toutes les plateformes qui utilisent le modèle MVC, l’interface utilisateur se compose d’une hiérarchie d’affichages. Un affichage dans le modèle MVC peut être perçu comme une hiérarchie d’affichage avec un affichage unique (appelé affichage racine) en haut de la hiérarchie et autant d’affichages enfants (appelés sous-affichages) en dessous. Dans iOS, la hiérarchie d’affichage de contenu d’un écran correspond au composant View du modèle MVC.

### <a name="controller"></a>Contrôleur

L’objet Controller est le composant qui relie tous les éléments ensemble et qui est représenté dans iOS par `UIViewController`. Nous pouvons le considérer comme le code de stockage d’un écran ou d’un ensemble d’affichages. L’objet Controller (ou contrôleur) est chargé d’écouter les demandes émises par l’utilisateur et de retourner la hiérarchie d’affichage appropriée. Il écoute les demandes émises à partir de l’affichage (clics de bouton, entrée de texte, etc.) et effectue le traitement, la modification de l’affichage et le rechargement de l’affichage appropriés. Le contrôleur est également responsable de la création ou de la récupération de l’objet Model à partir du magasin de données existant dans l’application et du remplissage de l’affichage avec ses données.

Les contrôleurs peuvent également gérer d’autres contrôleurs. Par exemple, un contrôleur peut charger un autre contrôleur s’il a besoin d’afficher un autre écran ou il peut gérer une pile de contrôleurs pour surveiller leur ordre et les transitions entre eux. Dans la section suivante, nous allons voir un exemple de contrôleur qui gère d’autres contrôleurs au fur et à mesure que nous présenterons un type spécial de contrôleur d’affichage iOS appelé *contrôleur de navigation*.

## <a name="navigation-controller"></a>Contrôleur de navigation

Dans l’application Phoneword, nous avons utilisé un *contrôleur de navigation* pour mieux gérer la navigation entre les multiples écrans. Le contrôleur de navigation est un `UIViewController` spécialisé représenté par la classe `UINavigationController`. Au lieu de gérer une seule hiérarchie d’affichage de contenu, le contrôleur de navigation gère d’autres contrôleurs d’affichage, ainsi que sa propre hiérarchie d’affichage de contenu spéciale sous la forme d’une barre d’outils de navigation qui inclut un titre, un bouton Précédent et d’autres fonctionnalités facultatives.

Le contrôleur de navigation est commun dans les applications iOS et assure la navigation pour des applications iOS de base comme l’application **Réglages**, comme l’illustre la capture d’écran ci-dessous :

 [![](hello-ios-multiscreen-deepdive-images/01.png "Le contrôleur de navigation assure la navigation des applications iOS comme l’application Réglages illustrée ici")](hello-ios-multiscreen-deepdive-images/01.png#lightbox)

Le contrôleur de navigation remplit trois fonctions principales :

-  **Il fournit des hooks pour la navigation en avant** : Le contrôleur de navigation utilise une métaphore de navigation hiérarchique où les hiérarchies d’affichage de contenu sont *envoyées* dans une *pile de navigation* . Vous pouvez considérer qu’une pile de navigation est comme une pile de cartes à jouer, dans laquelle seule la carte du dessus est visible, comme l’illustre le diagramme ci-dessous :  

    [![](hello-ios-multiscreen-deepdive-images/02.png "Ce diagramme illustre la navigation sous forme de pile de cartes")](hello-ios-multiscreen-deepdive-images/02.png#lightbox)


-  **Il fournit éventuellement un bouton Précédent** : Quand nous envoyons un nouvel élément dans la pile de navigation, la barre de titre peut afficher automatiquement un *bouton Précédent* qui permet à l’utilisateur de revenir en arrière. En appuyant sur le bouton Précédent, le contrôleur d’affichage actif est *retiré* de la pile de navigation et la précédente hiérarchie d’affichage de contenu dans la fenêtre se charge :  

    [![](hello-ios-multiscreen-deepdive-images/03.png "Ce diagramme illustre le retrait d’une carte de la pile")](hello-ios-multiscreen-deepdive-images/03.png#lightbox)


-  **Il fournit une barre de titre** : La partie supérieure du **contrôleur de navigation** est appelé *barre de titre*. Celle-ci est chargée d’afficher le titre du contrôleur d’affichage, comme l’illustre le diagramme ci-dessous :  

    [![](hello-ios-multiscreen-deepdive-images/04.png "La barre de titre est chargée d’afficher le titre du contrôleur de vue")](hello-ios-multiscreen-deepdive-images/04.png#lightbox)




### <a name="root-view-controller"></a>Contrôleur d’affichage racine

Un **contrôleur de navigation** ne gère pas une hiérarchie d’affichage de contenu, il n’a donc rien à afficher tout seul.
Un **contrôleur de navigation** est plutôt associé à un *contrôleur d’affichage racine* :

 [![](hello-ios-multiscreen-deepdive-images/05.png "Un contrôleur de navigation est associé à un contrôleur de vue racine")](hello-ios-multiscreen-deepdive-images/05.png#lightbox)

Le contrôleur d’affichage racine représente le premier contrôleur d’affichage dans la pile du **contrôleur de navigation** et la hiérarchie d’affichage de contenu du contrôleur d’affichage racine est la première hiérarchie d’affichage de contenu à charger dans la fenêtre. Si nous voulons placer toute notre application dans la pile du contrôleur de navigation, nous pouvons déplacer le Segue sans source dans le **contrôleur de navigation** et définir le contrôleur d’affichage de notre premier écran en tant que contrôleur d’affichage racine, comme nous l’avons fait dans l’application Phoneword :

 [![](hello-ios-multiscreen-deepdive-images/06.png "Le segue sans source définit le contrôleur de vue des premiers écrans en tant que contrôleur de vue racine")](hello-ios-multiscreen-deepdive-images/06.png#lightbox)

### <a name="additional-navigation-options"></a>Options de navigation supplémentaires

Le **contrôleur de navigation** est un moyen courant de gérer la navigation dans iOS, mais il n’est pas la seule option. Un [contôleur de barre d’onglets](~/ios/user-interface/controls/creating-tabbed-applications.md) peut diviser une application en différentes zones fonctionnelles. Un [contrôleur d’affichage fractionné](https://developer.xamarin.com/recipes/ios/content_controls/split_view/use_split_view_to_show_two_controllers) peut créer des affichages maître/détail et un [contrôleur de navigation par menu volant](http://components.xamarin.com/view/flyoutnavigation) crée une navigation que l’utilisateur peut balayer depuis le côté. Tous ces éléments peuvent être combinés avec un **contrôleur de navigation** pour présenter du contenu de manière intuitive.

## <a name="handling-transitions"></a>Gestion des transitions

Dans la procédure pas à pas Phoneword, nous avons traité la transition entre les deux contrôleurs d’affichage de deux manières différentes : tout d’abord avec un Segue de Storyboard, puis par programmation. Examinons ces deux options de plus près.

### <a name="prepareforsegue"></a>PrepareForSegue

Lorsque nous ajoutons un Segue avec une action **Afficher** au Storyboard, nous demandons à iOS d’envoyer le deuxième contrôleur d’affichage dans la pile du contrôleur de navigation :

 [![](hello-ios-multiscreen-deepdive-images/09.png "Définition du type de segue à partir d’une liste déroulante")](hello-ios-multiscreen-deepdive-images/09.png#lightbox)

L’ajout d’un Segue au Storyboard suffit à créer une transition simple entre les écrans. Si nous voulons passer des données entre les contrôleurs d’affichage, nous devons remplacer la méthode `PrepareForSegue` et gérer les données nous-mêmes :

```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);
    ...
}
```

iOS appelle `PrepareForSegue` juste avant que la transition ne se produise et passe le Segue que nous avons créé dans le Storyboard à la méthode.
À ce stade, nous devons définir manuellement le contrôleur d’affichage de destination du Segue. Le code suivant obtient un handle vers le contrôleur d’affichage de destination et le caste dans la classe adéquate, CallHistoryController, dans cet exemple :

```csharp
CallHistoryController callHistoryContoller = segue.DestinationViewController as CallHistoryController;
```

Enfin, nous passons la liste de numéros de téléphone (la partie Model) depuis `ViewController` vers `CallHistoryController` en définissant la propriété `PhoneHistory` du `CallHistoryController` sur la liste des numéros de téléphone composés :

```csharp
callHistoryContoller.PhoneNumbers = PhoneNumbers;
```

Le code complet permettant de passer des données à l’aide d’un Segue est le suivant :

```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    var callHistoryContoller = segue.DestinationViewController as CallHistoryController;

    if (callHistoryContoller != null) {
         callHistoryContoller.PhoneNumbers = PhoneNumbers;
    }
 }
```

### <a name="navigation-without-segues"></a>Navigation sans Segues

Le processus de transition entre le premier contrôleur d’affichage et le deuxième dans le code est le même qu’avec un Segue, mais plusieurs étapes doivent être effectuées manuellement.
Tout d’abord, nous utilisons `this.NavigationController` pour obtenir une référence au contrôleur de navigation dont la pile est celle où nous nous trouvons actuellement. Ensuite, nous utilisons la méthode `PushViewController` du contrôleur de navigation pour envoyer manuellement le contrôleur d’affichage suivant dans la pile, en passant le contrôleur d’affichage et une option d’animation de la transition (nous affectons la valeur `true`).

Le code suivant gère la transition entre l’écran Phoneword et l’écran de l’historique des appels :

```csharp
this.NavigationController.PushViewController (callHistory, true);
```

Pour pouvoir effectuer la transition vers le contrôleur d’affichage suivant, nous devons d’abord l’instancier manuellement à partir du Storyboard en appelant `this.Storyboard.InstantiateViewController` et en passant l’ID de Storyboard du `CallHistoryController` :

```csharp
CallHistoryController callHistory =
this.Storyboard.InstantiateViewController
("CallHistoryController") as CallHistoryController;
```

Enfin, nous passons la liste de numéros de téléphone (la partie Model) depuis `ViewController` vers `CallHistoryController` en définissant la propriété `PhoneHistory` du `CallHistoryController` sur la liste des numéros de téléphone composés, tout comme nous l’avons fait quand nous avons géré la transition avec un Segue :

```csharp
callHistory.PhoneNumbers = PhoneNumbers;
```

Le code complet pour la transition par programmation est le suivant :

```csharp
CallHistoryButton.TouchUpInside += (object sender, EventArgs e) => {
    // Launches a new instance of CallHistoryController
    CallHistoryController callHistory = this.Storyboard.InstantiateViewController ("CallHistoryController") as CallHistoryController;
    if (callHistory != null) {
     callHistory.PhoneNumbers = PhoneNumbers;
     this.NavigationController.PushViewController (callHistory, true);
    }
};
```

## <a name="additional-concepts-introduced-in-phoneword"></a>Autres concepts introduits dans Phoneword

L’application Phoneword a introduit plusieurs concepts qui ne sont pas traités dans ce guide. Ces concepts sont notamment :

-  **Création automatique de contrôleurs d’affichage** : Quand nous entrons un nom de classe pour le contrôleur d’affichage dans le **Panneau Propriétés**, le concepteur iOS vérifie si cette classe existe, puis génère la classe de stockage du contrôleur d’affichage pour nous. Pour plus d’informations sur ce concept et d’autres fonctionnalités du concepteur iOS, reportez-vous au guide de [présentation du concepteur iOS](~/ios/user-interface/designer/introduction.md).
-  **Contrôleur d’affichage Table** : `CallHistoryController` est un contrôleur d’affichage Table. Un contrôleur d’affichage Table contient un affichage Table, l’outil de disposition et d’affichage de données le plus courant dans iOS. Les tables dépassent le cadre du présent guide. Pour plus d’informations sur les contrôleurs d’affichage Table, reportez-vous au guide d’[utilisation des tables et cellules](~/ios/user-interface/controls/tables/index.md).
-   **ID de Storyboard** : La définition de l’ID de Storyboard crée une classe de contrôleur d’affichage en Objective-C, qui contient le code-behind du contrôleur d’affichage dans le Storyboard. Nous utilisons l’ID de Storyboard pour trouver la classe Objective-C et instancier le contrôleur d’affichage dans le Storyboard. Pour plus d’informations sur les ID de Storyboard, reportez-vous au guide de [présentation des Storyboards](~/ios/user-interface/storyboards/index.md).


## <a name="summary"></a>Récapitulatif

Félicitations ! Vous avez terminé votre première application iOS multi-écran.

Dans ce guide, nous avons présenté le modèle MVC et nous l’avons utilisé pour créer une application multi-écran. Nous avons également examiné les contrôleurs de navigation et leur rôle dans la navigation iOS. Vous avez maintenant acquis les bases solides dont vous avez besoin pour commencer à développer vos propres applications Xamarin.iOS.

Maintenant, apprenons à générer des applications multiplateforme avec Xamarin à l’aide des guides de [présentation du développement mobile](~/cross-platform/get-started/introduction-to-mobile-development.md) et de [génération d’applications multiplateforme](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md).


## <a name="related-links"></a>Liens associés

- [Hello, iOS (exemple)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [Lignes directrices de l’interface utilisateur iOS](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [Portail de provisionnement iOS](https://developer.apple.com/ios/manage/overview/index.action)
