---
title: Introduction au développement de jeux avec Xamarin
description: La nature du développement de jeux peut varier considérablement le développement d’autres types d’applications. Cet article est une introduction au développement de jeux en intégrant des technologies qui peuvent être utilisées avec Xamarin.iOS et Xamarin.Android. Il fournit une présentation de haut niveau de la façon dont sont effectuées les jeux et un échantillonnage de technologies disponible pour une utilisation avec Xamarin.iOS et Xamarin.Android.
ms.prod: xamarin
ms.assetid: 0E3CDCD2-FBE4-49F5-A70E-8A7B937BAF1D
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: b2df6d431004bbfa140b6cae1d069404af92c1df
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-game-development-with-xamarin"></a>Introduction au développement de jeux avec Xamarin

_La nature du développement de jeux peut varier considérablement le développement d’autres types d’applications. Cet article est une introduction au développement de jeux en intégrant des technologies qui peuvent être utilisées avec Xamarin.iOS et Xamarin.Android. Il fournit une présentation de haut niveau de la façon dont sont effectuées les jeux et un échantillonnage de technologies disponible pour une utilisation avec Xamarin.iOS et Xamarin.Android._

Développement de jeux peut être très intéressant, particulièrement donné comment il peut être facile à publier votre travail sur des plates-formes mobiles. Cet article traite des concepts et technologies de développement de jeux qui vous aideront à créer des jeux, si votre objectif consiste à créer un AAA de haute qualité jeu ou simplement au programme pour vous amuser.

Cet article couvre les rubriques suivantes :

- **Jeu et les concepts de programmation hors jeu** – nous aborderons certains concepts qui sont uniques pour le développement d’un jeu, ou sont partagés avec d’autres types de développement mais méritent accentuation ici en raison de leur importance.
- **Équipe de développement de jeux** – cette section présente les différents rôles dans une équipe de développeurs de jeu.
- **Création d’une idée du jeu** – cette section permettre vous aider à créer un nouveau concept de jeu – la première étape lors de l’élaboration d’une nouvelle partie.
- **Technologie de développement de jeux** – ici, nous allons répertorier quelques-unes des technologies inter-plateformes disponibles qui peuvent améliorer votre productivité en tant que jeu développeur.


# <a name="game-vs-non-game-programming-concepts"></a>Jeu vs. Concepts de programmation hors jeu

Les programmeurs de déplacement dans le développement de jeux sont souvent confrontés à des nouveaux concepts et les modèles de développement. Cette section présente une vue d’ensemble de certaines de ces concepts.


## <a name="the-game-loop"></a>La boucle de jeu

Un jeu standard requiert le déplacement des constantes ou modification de conditions sur l’écran en réponse à l’intervention de l’utilisateur et une logique de jeu automatique. Cela est possible via ce qui est généralement appelé une *boucle game*. Une boucle de jeu est un type d’instruction (par exemple, une boucle while) qui s’exécute sur un très fréquent, telles que les 30 ou 60 de bouclage *images par seconde*.

Voici un diagramme d’une simple boucle jeu :

![](images/image1.png "Il s’agit d’un diagramme d’une simple boucle de jeu")

Les technologies que nous parlerons ci-dessous abstraire la boucle while réelle, mais en dépit de cette abstraction, le concept de mises à jour de chaque image seront présent.

Performances du code peut prioritaires dans la même la plus simple de jeux. Par exemple : une fonction qui prend de 10 millisecondes pour exécuter peut avoir un impact significatif sur les performances d’un jeu, en particulier si elle est appelée plusieurs fois par frame. Si votre jeu s’exécute à 30 images par seconde cela signifie que chaque image doit s’exécuter en millisecondes sous 33. En revanche, une telle fonction même peut-être pas perceptible si elle s’exécute uniquement en réponse à un clic de bouton dans une application hors jeu.

Les types courants de logique qui peuvent être effectuée chaque image sont les suivantes :

- **La lecture d’entrée** – le jeu devez vérifier si l’utilisateur interagisse avec le jeu en vérifiant le matériel d’entrée, tels que l’écran tactile, un clavier, une souris ou un contrôleur de jeu.
- **Le déplacement des** – objets le déplacer d’un emplacement vers un autre déplacera généralement une très petite quantité chaque frame pour donner une impression de mouvement fluide.
- **Collision** : de nombreux jeux requièrent le test fréquentes de différents objets sont chevauchement ou intersection. Nous aborderons la collision de manière plus approfondie dans une section plus loin dans cet article. Le déplacement et la collision peuvent être gérées par un système de simulation physique dédié.
- **La vérification des conditions de jeu spécifiques** : l’état d’un jeu peut être contrôlé à certaines conditions, telles que si le joueur a gagné suffisamment de points ou si les temps imparti est épuisée.
- **Comportement de AI** – logique de chaque image qui peut être utilisée pour contrôler le comportement des objets qui ne sont pas contrôlées par le lecteur, tel que le patrouiller d’un menaces ou le déplacement des pilotes adversaire autour d’un ovale.
- **Rendu** – la plupart des jeux va mettre à jour ce qui est affiché dans l’écran chaque frame. Cette opération peut être effectuée en réponse aux modifications qui ont l’impact sur le jeu (par exemple, le déplacement d’un niveau d’un caractère) ou simplement fournir visual polonais (par exemple, les chutes de neige ou des icônes animées).

Gardez à l’esprit que la plupart des activités répertoriées ci-dessus peuvent modifier l’état de l’application, alors que de nombreuses applications hors jeu ont tendance à changer d’état en réponse aux événements déclenchés.


## <a name="content-loading-and-unloading"></a>Chargement de contenu et de déchargement

Contenu manuellement lors du chargement et déchargement (ou suppression) peut-être être nécessaires en fonction de la technologie que vous utilisez dans le développement. Manuellement le chargement et déchargement de ressources peuvent être nécessaires pour plusieurs raisons :

 - Ressources peuvent prendre beaucoup de temps par rapport à la longueur d’une seule image de charge. Certains des éléments peuvent même avoir les secondes à charger, ce qui risque de perturber gravement l’expérience si chargé mid jeu. Si le temps de chargement est particulièrement long (par exemple, plus d’une ou deux secondes), vous souhaiterez afficher une animation du chargement d’écran de barre de progression.
 - Ressources peuvent consommer une grande quantité de RAM, nécessitant une gestion active de ce qui est chargé pour s’ajuster à celle fournie par des plateformes cibles du jeu.
 - Jeux devrez peut-être afficher plus de ressources ne tient pas dans la mémoire RAM. Jeux de « Ouvrir World » incluent souvent des environnements de grande taille quels lecteurs peuvent naviguer en toute transparence – avec aucun écran de chargement. Dans ce cas, vous devrez créer un système personnalisé pour le contenu de diffusion en continu dans et la gestion de la mémoire.

Formats de fichiers personnalisés peut-être le traitement au moment du chargement, nécessitant le code de chargement personnalisé.


## <a name="math"></a>Math

De nombreux jeux requièrent mathématiques plus avancées que les applications hors jeu. Bien entendu, le niveau de mathématiques varie selon la complexité du jeu. En général les jeux 3D nécessite plus mathématiques à 2D. Heureusement, vous pourrez toujours prise en main des jeux simples et en savoir plus que vous avancez. Développement de jeux peut être un excellent moyen de savoir mathématiques !

Si vous êtes familiarisé avec le plan cartésien : qui est à l’aide de coordonnées X et Y afin de positionner les objets, puis vous en savez suffisamment pour commencer avec le développement de jeux. Le code suivant illustre un plan cartésien avec positif Y pointant vers le haut :

![](images/image2.png "Cet exemple montre un plan cartésien avec positif Y pointant vers le haut")

> [!IMPORTANT]
> Certains moteurs/API utilisent un système de coordonnées où augmenter la valeur de Y d’un objet sera déplacez-le vers le bas, tandis que d’autres systèmes utilisent un système de coordonnées où positif Y est. Gardez à l’esprit si vous êtes en déplacement entre les systèmes.
Fonctions trigonométriques (telles que sinus et cosinus) sont couramment utilisées dans les jeux 2D qui implémentent aucune forme de rotation.



Si vous envisagez de rendre un jeu 3D vous aurez probablement besoin de se familiariser avec les concepts d’algèbre linéaire (pour la rotation et de déplacement dans l’espace 3D), ainsi que certains calcul (pour l’implémentation d’accélération).


## <a name="content-pipelines"></a>Pipelines de contenu

Le terme *contenu pipeline* fait référence au processus qui permet d’obtenir à partir de son format lorsque créé (par exemple, un fichier d’image .png) un fichier dans son format final lorsqu’il est utilisé dans un jeu. Le format fin varie sur lequel le type de contenu est utilisé, ainsi que de laquelle la technologie est utilisée pour présenter le contenu.

Certains contenus pipelines peuvent être très rapide et ne requièrent aucun effort manuel. Par exemple, la plupart des API et les moteurs de jeu de peut charger le format de fichier .png dans son format non traité. En revanche, des formats plus complexes (tels que les modèles 3D) devront peut-être être traités dans un autre format avant leur chargement, et ce traitement peut prendre un certain temps selon la taille et la complexité de l’élément multimédia.


# <a name="game-development-teams"></a>Équipes de développement de jeux

Développement de jeux introduit de nouveaux rôles et les titres pour les personnes impliquées dans le processus. La plupart des développeurs jeu ne sont pas en mesure de satisfaire un large éventail de compétences nécessaires pour libérer un jeu complet, pour un nombre de disciplines existe. Gardez à l’esprit qu’il ne s’agit pas d’une liste complète des domaines de développement – quelques-unes des plus courants.

- **Programmeur** : la plupart des personnes qui consultent cet article appartiennent à cette catégorie. Le rôle d’un programmeur dans le développement de jeux est similaire à un programmeur dans une application hors jeu. Comprend l’écriture de logique pour contrôler le flux d’un jeu de développement de systèmes pour les tâches courantes dans le contexte d’un projet donné, ajout et affichage du contenu et – naturellement : résolution des bogues.
- **Artiste 2D** – artistes 2D sont chargées de créer *actifs 2D*. Notamment les fichiers image pour l’interface graphique utilisateur, les particules, les environnements et les caractères du jeu. Si le jeu que vous développez est 3D, les artistes 2D ne peuvent pas être chargés pour les environnements et des caractères. Vous pouvez trouver libre art pour votre jeu à [ http://opengameart.org/ ](http://opengameart.org/) .
- **Artistes 3D** – artistes 3D sont chargées de créer *actifs 3D*. Cela comprend les modèles 3D pour les environnements, les caractères et les propriétés (fournitures plantes et autres objets inanimés). Certaines équipes de différencier la 3D et des animations 3D en fonction de la taille de l’équipe. Vous pouvez trouver libre art 3D pour votre jeu à [ http://opengameart.org/ ](http://opengameart.org/) .
- **Concepteur de jeu** – jeu concepteurs sont responsables de la définition du mode de lecture du jeu. Cela peut inclure des décisions de haut niveau tels que le paramètre de la partie, l’objectif global du jeu, et la façon dont un lecteur passe par le biais du jeu. Les concepteurs de jeux peuvent également être impliqués dans les décisions très détaillées comme entrée de mappage à des actions, définition des coefficients de mouvement ou au niveau des sauvegardes et la conception de disposition de niveau. N’oubliez pas que le terme *concepteur* peut faire référence à un concepteur de jeu ou un concepteur visuel en fonction du contexte.
- **Son concepteur** – son est chargés pour les éléments audio d’un jeu. Certaines équipes peuvent distinguer les personnes chargées de créer des effets sonores et les compositeurs, tandis que les plus petites équipes peuvent avoir une seule personne responsable de tous les fichiers audio.


# <a name="creating-a-game-idea"></a>Création d’une idée du jeu

Conception d’un jeu peut-être apparaître pour être faciles à faire : une fois toutes les la seule exigence est « faire quelque chose d’amusant. » Malheureusement, de nombreux développeurs se retrouvent à perte lorsqu’il s’agit de temps pour créer une idée de lancer le développement.

La discipline de conception de jeu n’est pas facilement expliquée et nécessite pratique pour améliorer tout comme l’art ou est de programmation, mais cette section peut vous aider à démarrer le chemin d’accès.

Nouveaux développeurs doivent commencer. Il peut être difficile de résister à la tentation de refaire un jeu de vidéo volumineux et modern, mais plus petits jeux peuvent être un meilleur environnement de formation et la progression plus rapide pour une expérience plus récompense.

De nombreux jeux, à la fois à des fins de jeux d’apprentissage, ainsi que commerciales, commencent en tant qu’amélioration ou modification d’un jeu existant. Permet de générer des idées est d’examiner d’autres jeux d’inspiration. Par exemple, vous pouvez envisager un jeu que vous aimez personnellement et essayez d’identifier les caractéristiques de l’exécution du jeu rendent agréable. Il peut être exploration, la maîtrise des mécanismes du jeu, ou mise en œuvre un récit. N’oubliez pas de prendre en compte les jeux « rétro » ainsi lors de la recherche de nouvelles idées.

Une autre technique pour la génération de nouvelles idées consiste à prendre en compte un genre spécifique, tels que les jeux de puzzle, les jeux de stratégie ou platformers. Un genre familier pour le développeur peut fournir un bon point de départ.

Données essentielles au retraçage jeux existants est également une expérience de formation, bien que cela peut limiter les viabilité commerciale du produit fini. Le processus de création d’un jeu, même s’il qui est un clone précis, fournit une expérience de formation précieuse.


# <a name="game-development-technology"></a>Technologie de développement de jeux

Les développeurs qui utilisent Xamarin.Android et Xamarin.iOS ont un large éventail de technologies disponibles pour vous aider dans le développement de jeux. Cette section décrit certaines des solutions les plus populaires inter-plateformes.


## <a name="cocossharp"></a>CocosSharp

CocosSharp est une version open source, multiplateforme du moteur Cocos 2D jeu. Le moteur fournit l’accès à Android, iOS, Mac OS X, le bureau Windows, Windows RT et Windows Phone.

CocosSharp se concentre sur un programmeur simple API pour le développement de jeux 2D. L’augmentation de jeu sur des appareils mobiles a permis de reignite la popularité du développement de jeux 2D faire de la technologie de viable CocosSharp pour passe-temps et commerciales projets similaires. Il est fourni en tant que fichiers .dll ou de code source (qui peuvent être obtenus via NuGet), mais elle n’offre pas un éditeur visuel ; Par conséquent, toute interaction avec le moteur CocosSharp requiert une connaissance de la programmation.

Pour commencer avec CocosSharp, consultez notre [CocosSharp guides](~/graphics-games/cocossharp/index.md).

Le jeu en colère araignées géantes est créé avec CocosSharp, et il peut être un bon point de départ si vous avez besoin d’un jeu en cours d’exécution pour plusieurs plateformes :

![](images/image3.png "Le jeu en colère araignées géantes a été créé avec CocosSharp")

Vous pouvez le télécharger et obtenir plus d’informations sur la [AngryNinjas Github page](https://github.com/xamarin/AngryNinjas).


## <a name="monogame"></a>MonoGame

MonoGame est une open source, entre la version de la plateforme de Microsoft XNA API. MonoGame peut être utilisé pour créer des jeux pour iOS, Android, Mac OS X, Linux, Windows, Windows RT et Windows Phone.

Contrairement à CocosSharp, MonoGame est techniquement pas un moteur de jeu, mais plutôt un développement de jeux API. Cela signifie que l’utilisation des MonoGame requiert directement la gestion des objets de jeu, manuellement dessiner des objets et l’implémentation des objets courants tels que les appareils photo et *graphiques de scène* (la hiérarchie de l’enfant parent entre les objets de jeu). Pour aider à comprendre cette distinction, considérez que CocosSharp s’appuie sur MonoGame. MonoGame généralise certains de la technologie spécifique à la plateforme, tels que les graphiques, le rendu et audio, tandis que CocosSharp fournit du code pour l’organisation et l’implémentation de la logique de jeu.

MonoGame n’offre pas un environnement de développement visual standard pour l’utilisation des MonoGame requiert la base de connaissances de programmation.

Voici quelques exemples importants des jeux à l’aide de MonoGame :

FEZ :

![](images/image7.png "FEZ")

Bastion :

![](images/image8.jpg "Bastion")

Pour commencer à travailler avec MonoGame, rendez-vous sur notre [MonoGame Guides](~/graphics-games/monogame/index.md).


## <a name="urhosharp"></a>UrhoSharp

UrhoSharp est un inter-plateformes principales 2D et 3D moteur peut être utilisé pour créer des scènes 3D et 2D animées pour vos applications à l’aide de géométries, matériaux, lumières et appareils photo.

![](images/urhosharp.gif "UrhoSharp est un moteur 2D et 3D du haut niveau des inter-plateformes qui peut être utilisé pour créer les scènes animées 2D et 3D")

Extraire le [UrhoSharp Guides](~/graphics-games/urhosharp/index.md) pour commencer.

## <a name="additional-technology"></a>Technologie supplémentaire

Les technologies de mise en surbrillance ci-dessus est uniquement un échantillon des technologies disponibles. Autres technologies notables sont les suivantes :

- **Kit de Sprite** – Xamarin prend en charge Sprite Kit jeu framework d’Apple, qui vous donne accès à toutes les fonctionnalités de l’API native. Sprite Kit étant une technologie créée par Apple, il fournit une intégration étroite avec le reste de l’écosystème d’iOS. Bien entendu, Sprite Kit n’est pas inter-plateformes afin qu’il ne peut pas être utilisé sur Android. Pour plus d’informations sur l’utilisation de Sprite Kit, consultez le billet :  [http://blog.xamarin.com/make-games-with-xamarin.ios-and-sprite-kit/](http://blog.xamarin.com/make-games-with-xamarin.ios-and-sprite-kit/)
- **Kit de scène** – Xamarin prend également en charge pour le framework de scène Kit d’Apple, ce qui simplifie la mise en œuvre des graphiques 3D dans des applications iOS. Kit de scène est également technologie fournie par Apple, de sorte que l’intégration et les considérations spécifiques à une plateforme mentionnées ci-dessus pour Sprite Kit. Pour plus d’informations sur le Kit de la scène, consultez le billet : [http://blog.xamarin.com/3d-in-ios-8-with-scene-kit/](http://blog.xamarin.com/3d-in-ios-8-with-scene-kit/)
- **OpenTK –** OpenTK (qui correspond à la boîte à outils ouverte) fournit des accès de bas niveau OpenGL pour iOS et Apple Mac matériel. Pour plus d’informations sur OpenTK, consultez la page principale à :  [http://www.opentk.com/](http://www.opentk.com/)


# <a name="summary"></a>Récapitulatif

Cet article aborde les principaux concepts du développement de jeux et fournit des informations sur la prise en main apporter votre premier jeu. Une fois que vous avez fini de cet article, les étapes suivantes sont à choisir la technologie et commencer à travailler sur notre série de didacticiels liés dans les sections appropriées ci-dessus.

## <a name="related-links"></a>Liens associés

- [Repères de CocosSharp](~/graphics-games/cocossharp/index.md)
- [Repères de MonoGame](~/graphics-games/monogame/index.md)
- [Repères de UrhoSharp](~/graphics-games/urhosharp/index.md)
