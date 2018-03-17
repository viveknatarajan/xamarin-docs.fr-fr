---
title: "Présentation des Concepts de SiriKit"
description: "Cet article aborde les concepts clés qui seront nécessaires pour travailler avec SiriKit dans une application Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 99EC5C1E-484F-4371-8555-58C9F60DE37F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 67635b6a04579246467184cdff8d9f277b36ecc4
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/17/2018
---
# <a name="understanding-sirikit-concepts"></a>Présentation des Concepts de SiriKit

_Cet article aborde les concepts clés qui seront nécessaires pour travailler avec SiriKit dans une application Xamarin.iOS._


Nouveau pour iOS 10, SiriKit permet à une application Xamarin.iOS fournir des services qui sont accessibles à l’utilisateur à l’aide de Siri et l’application de cartes sur un appareil iOS. Cette fonctionnalité est fournie dans un ou plusieurs Extension d’application à l’aide de la nouvelle **intentions** et **intentions UI** infrastructures.

SiriKit permet à une application iOS fournir des services qui sont accessibles à l’utilisateur à l’aide de Siri et l’application de cartes sur un appareil iOS à l’aide des Extensions d’application et les nouveaux **intentions** et **intentions UI** infrastructures.

Siri fonctionne avec le concept de **domaines**, groupes de connaître les actions pour les tâches associées. Chaque interaction de l’application avec Siri doit appartenir à l’un de ses domaines de service connu comme suit :

- Audio ou vidéo.
- Un cas de réservation.
- La gestion de sommeil.
- Gestion des messages.
- Recherche des photos.
- Envoyer ou recevoir des paiements.

Lorsque l’utilisateur effectue une demande de Siri impliquant un des services de l’Extension application, SiriKit envoie l’extension un **intention** objet qui décrit la demande de l’utilisateur, ainsi que toutes les données de prise en charge. l’Extension d’application, puis génère les **réponse** de l’objet pour la donnée **intention**, détaillant la façon dont l’extension peut gérer la demande.

## <a name="the-intents-and-intents-ui-extensions"></a>Les modes et les Extensions de l’interface utilisateur d’intentions

Siri et l’application de cartes interagissent avec les services de l’application via deux types d’Extensions d’application :

- **Extension des intentions** -fournit des Siri et des cartes avec l’application de contenu et effectue les tâches requises pour répondre à toutes les modes pris en charge.
- **Extension de l’interface utilisateur intentions** -fournit une interface utilisateur personnalisée qui s’affichera pour le contenu de l’application à l’intérieur de Siri ou mappages.

L’application doit fournir une Extension de modes pour prendre en charge SiriKit et il est chargé pour fournir des informations Siri et des mappages peuvent présenter à l’utilisateur et la gestion des objectifs.

Création d’une Extension de l’interface utilisateur intentions est facultative dans la mesure où Siri généralement gère toutes les interactions de l’utilisateur a une interface d’utilisateur standard, prédéfini pour présenter des informations dans chacun des domaines pris en charge. En fournissant une Extension de l’interface utilisateur intentions, l’application peut utiliser le **l’interface utilisateur de l’intention de** framework pour présenter une riche, personnalisé Interface utilisateur présentant l’homologation de l’application et des informations supplémentaires.

## <a name="siri-and-the-maps-app-role"></a>Siri et le rôle d’application de mappages

Requêtes parlée de l’utilisateur sont traités et sémantiquement analysées par Siri qui transforme les intentions utilisables capable de gérer les Extensions de l’intention de ces demandes de la langue.

Maps utilise les Extensions d’intention de l’application pour afficher des informations dans l’interface de la carte en réponse aux actions de l’utilisateur. Telles que la demande à proximité de restaurants ou obtenir un restaurant de l’application passe en revue.

Siri et cartes de gérer toutes les interactions de l’utilisateur et affichent les résultats à l’aide de l’interface du système standard. Le rôle d’application extensions est principalement de fournir les données qui s’affiche. Si vous le souhaitez, l’application peut fournir une Extension de l’interface utilisateur intentions et présenter une interface utilisateur personnalisée pour des raisons de l’interface du système par défaut.

## <a name="interacting-with-siri-via-sirikit"></a>Interaction avec Siri Via SiriKit

Cette section présente une vue d’ensemble de la manière dont SiriKit permet à l’utilisateur d’interagir avec l’application à l’aide de Siri. Cet exemple, nous utiliserons l’application MonkeyChat fausse :

[![](understanding-sirikit-images/monkeychat01.png "L’icône MonkeyChat")](understanding-sirikit-images/monkeychat01.png#lightbox)

MonkeyChat conserve son propre contact Carnet d’adresses de vos amis de l’utilisateur, chacune associée à un nom de l’écran (par exemple, Bobo par exemple) et permet à l’utilisateur d’envoyer des conversations de texte à chaque ami par leur nom de l’écran.

Il existe plusieurs façons que l’utilisateur peut initier une interaction avec l’application, dans la mesure où différentes personnes rendrait la même demande de nombreuses formes.

Par exemple, si l’utilisateur veut envoyer un message à leur Bobo friend, peut avoir la conversation suivante avec Siri :

_Utilisateur : Bonjour Siri, envoyer un message MonkeyChat._<br />
_Siri : auquel ?_<br />
_Utilisateur : Bobo._<br />
_Siri : Que voulez-vous dire à Bobo ?_<br />
_Utilisateur : Envoyez banane plus._<br />

Une autre personne peut apporter la même requête avec une autre conversation :

_Utilisateur : Envoyer un message à Bobo sur MonkeyChat._<br />
_Siri : Que voulez-vous dire à Bobo ?_<br />
_Utilisateur : Envoyez banane plus._<br />

Et un autre utilisateur peut effectuer une requête encore plus courte :

_Utilisateur : MonkeyChat Bobo envoyez banane plus._<br />
_Siri : Ok, envoi envoyez plus banane à Bobo sur Monkeychat._<br />

Ou même de faire la même requête dans une autre langue :

_Utilisateur : MonkeyChat Bobo s'il vous plaît envoyer plus de bananes._<br />
_Siri : Oui, envoyer d’envoi message s'il vous plaît plus de bananes à Bobo sur Monkeychat._<br />

Encore un autre utilisateur peut être très détaillé dans leur conversation :

_Utilisateur : Bonjour Siri, pouvez Veuillez ne me plaisir et lancer l’application MonkeyChat pour envoyer un texte avec le message envoyez banane plus._<br />
_Siri : auquel ?_<br />
_Utilisateur : Mon meilleures pal Bobo._<br />

En outre, il existe de nombreuses façons Siri peut répondre à une requête, certaines en fonction de la façon dont la demande a été effectuée :

- **En maintenant le bouton accueil** -Siri fournit davantage de réponses visual avec commentaires verbaux limité.
- **Par « Hey Siri »** -Siri sera plus verbale et fournir des réponses visual moins.

Siri est également optimisé pour répondre aux besoins d’accessibilité de l’utilisateur et interagir et répondre en fonction de ces besoins.

Quelle que soit la façon dont une demande est faite ou comment Siri répond à la demande, Siri gère la conversation avec l’utilisateur et l’application (via ses Extensions) fournit les fonctionnalités.

Lorsque l’utilisateur effectue une demande verbale de Siri, voici les étapes qui suivre Siri :

[![](understanding-sirikit-images/monkeychat02.png "Les étapes qui suivre Siri")](understanding-sirikit-images/monkeychat02.png#lightbox)

1. Tout d’abord, Siri prend le fichier audio de l’utilisateur **vocale** et le convertit en texte.
2. Ensuite, le texte est converti en un **intention**, un structuré en représentation sous forme de demande de l’utilisateur.
3. Selon l’intention, Siri prendra **Action** pour effectuer la demande de l’utilisateur.
4. Enfin, présente Siri **réponses** (visual et verbale) à l’utilisateur en fonction de l’Action effectuée.

Il existe trois manières principales que l’application puisse participer à la conversation de l’utilisateur avec Siri :

[![](understanding-sirikit-images/monkeychat03.png "Les trois méthodes principales que l’application peut prendre part à la conversation d’utilisateurs avec Siri")](understanding-sirikit-images/monkeychat03.png#lightbox)

1. **Vocabulaire** -Voici comment l’application indique Siri les mots qu’il a besoin pour interagir avec elle.
2. **Logique de l’application** - Voici les actions et les réponses que l’application doit prendre selon donné intentions.
3. **Interface utilisateur** -c’est l’interface utilisateur personnalisée facultative que l’application peut donner ses réponses dans.

### <a name="example"></a>Exemple

Étant donné les informations ci-dessus, examinez l’interaction de la conversation suivante avec l’application MonkeyChat :

_Utilisateur : Bonjour Siri, envoyer un message à Bobo sur MonkeyChat._<br />
_Siri : Que voulez-vous dire à Bobo ?_<br />
_Utilisateur : Envoyez banane plus._<br />

Le premier rôle de l’application prend à la conversation doit comprendre Siri vocale de l’utilisateur :

[![](understanding-sirikit-images/monkeychat04.png "Description des Siri vocal des utilisateurs")](understanding-sirikit-images/monkeychat04.png#lightbox)

Siri ne possède pas le nom « Bobo » dans sa base de données, mais l’application et partage ces informations avec Siri via son vocabulaire. L’application permet également de Siri reconnaître que Bobo un destinataire, car il leur spécifié à Siri comme un *Contact*.

Siri sait que plus est requise pour envoyer un message à un destinataire uniquement, donc il vérifie rapidement avec l’Extension d’application pour voir si un message nécessite de contenu. Étant donné que MonkeyChat est le cas, Siri répondra à l’utilisateur à la question : *« Que voulez-vous dire à Bobo ? »*

Dans l’exemple ci-dessus, l’utilisateur a répondu, *« Veuillez envoyer plus banane »*, qui Siri sera regrouper dans un texte structuré **intention**:

[![](understanding-sirikit-images/monkeychat05.png "Siri regroupera la réponse de l’utilisateur dans un but structuré")](understanding-sirikit-images/monkeychat05.png#lightbox)

L’objectif structurée contient les informations suivantes :

- **Domaine :** Messages
- **Objectif :** sendMessage
- **Destinataire :** Bobo
- **Contenu :** envoyez banane plus

Chaque domaine a en tant qu’ensemble de savoir *Actions* qui peut être effectuée au sein de celles-ci et basé sur le domaine et l’Action, de nombreux paramètres peuvent être inclus dans le but de zéro est envoyée à l’application.

L’objectif est ensuite envoyé à l’Extension d’application pour le traitement. Par conséquent l’application de l’intention de traitement, génère une **IntentResponse** qui est livré avec l’intention et inclure des paramètres qui décrivent ce que l’application de l’application se fait avec l’intention.

Chaque IntentResponse inclut également un **Code de réponse** qui indique Siri si l’application a été en mesure d’effectuer la demande ou non. Certains domaines ont les codes de réponse d’erreur très spécifique qui peuvent être envoyés, ainsi.

Enfin, le IntentResponse inclura un `NSUserActivity` (telles que celles utilisées pour prendre en charge remette). Le `NSUserActivity` permet de lancer l’application si la réponse en a besoin pour quitter l’environnement Siri et entrer dans l’application pour la terminer. 

Siri générera automatiquement une `NSUserActivity` pour démarrer l’application et la collecte, où l’utilisateur s’est arrêté dans l’environnement Siri. Toutefois, l’application peut fournir sa propre `NSUserActivity` avec personnalisé d’informations, si nécessaire.

Une fois que l’application a traité l’intention et a retourné une réponse à Siri, il présente ensuite les résultats à l’utilisateur (verbalement et visuellement) :

[![](understanding-sirikit-images/monkeychat06.png "Les résultats présentés à l’utilisateur à la fois verbalement et visuellement")](understanding-sirikit-images/monkeychat06.png#lightbox)

Siri a réponse intégré plusieurs Interfaces utilisateur pour chacun des domaines disponibles pour l’application. Toutefois, étant donné que MonkeyChat a fourni une Extension facultative d’intention de l’interface utilisateur, il est utilisé pour présenter les résultats de la conversation à l’utilisateur dans l’exemple ci-dessus.

## <a name="the-intent-lifecycle"></a>Le cycle de vie Intent

Il existe trois tâches principales que l’Extension d’application doit effectuer lors du traitement de modes :

[![](understanding-sirikit-images/monkeychat07.png "Le cycle de vie Intent")](understanding-sirikit-images/monkeychat07.png#lightbox)

1. L’application doit **résoudre** chaque paramètre d’un événement. Par conséquent, l’application appelle résoudre plusieurs fois (une fois par chaque paramètre) et parfois plusieurs fois sur le même paramètre jusqu'à ce que l’application et l’utilisateur sont d’accord sur ce qui est demandé.
2. L’application doit **confirmer** qu’il peut gérer l’intention demandée et informer Siri le résultat attendu.
3. Enfin, l’application doit **gérer** l’intention et effectuer les étapes pour obtenir le résultat demandé.

### <a name="the-resolve-stage"></a>La phase de résolution

La phase de résolution permet de comprendre les valeurs que l’utilisateur a fourni et garantit que ce que l’utilisateur est réellement signifiait que se passe-t-il lorsque l’intention est traitée par l’application Siri. 

Cette étape fournit également une opportunité pour l’application influencer le comportement de Siri pendant la conversation avec l’utilisateur. Pour ce faire, l’application fournit un **réponse de résolution**. Il existe un nombre de réponse prédéfini pour les différents types de données comprenant Siri.

La plus courante réponse de résolution à partir de l’application sera **réussite**, ce qui signifie que l’application mis en correspondance avec l’élément spécifique de données à partir d’un paramètre (par exemple, le nom de l’écran utilisateur) d’une information qu’il connaît.

Il peut arriver lorsque l’application a besoin pour confirmer qu’une demande donnée correspond à l’information correcte qu’il connaît. Dans ces cas, il enverra un **ConfirmationRequired** réponse à poser une question Oui ou non à l’utilisateur telles que *« Envoi de message à Bobo le grand ? »*

Il existe peut-être d’autres cas où l’application nécessite l’utilisateur de choisir à partir d’une liste courte d’options. Dans ce cas, l’application fournit un **ambiguïté** réponse avec une liste d’options de deux à dix pour l’utilisateur de choisir à partir de telles que : 

```csharp
Who do you want to message?

* Bobo the Great
* Bobo Jr.
* Little Bobo
```

Siri gère l’utilisateur qui effectue la sélection, ou en interagissant avec l’UI Siri verbalement, et le résultat sera être envoyé vers l’application.

Dans d’autres cas, il ne peut pas y avoir suffisamment d’informations pour l’application pour résoudre le paramètre ou il peut y avoir trop de correspondances à résoudre à l’aide de la levée de l’ambiguïté (par exemple, aux 80 utilisateurs avec Bobo dans leur nom). Dans ce cas, l’application envoie un **NeedsMoreDetails** réponse et Siri invite l’utilisateur à être plus précis.

Si l’utilisateur n’a pas fourni une valeur qui est nécessaire à l’intention de processus, il peut envoyer un **NeedsValue** réponse avoir Siri inviter l’utilisateur à la valeur.

Si l’application ne prend pas en charge une valeur que l’utilisateur a donné pour un paramètre spécifique, il peut envoyer le **UnsupportedWithReason** réponse à fournir une raison pourquoi la valeur n’a pas été pris en charge. Siri puis invite l’utilisateur pour une valeur et étant donné les la raison pour laquelle il est nécessaire.

Enfin, utiliser le **NotRequired** réponse pour indiquer à Siri que l’application ne nécessite pas une valeur pour un paramètre donné. Si l’utilisateur fournit un tout de même, il sera tout simplement ignoré par Siri.

### <a name="the-confirm-stage"></a>L’étape de confirmation

L’étape de confirmation a deux objectifs :

- Pour indiquer le résultat attendu de gérer une intention afin que Siri peut indiquer à l’utilisateur ce qui va se produire à Siri.
- Fournit une vérification d’opportunité les états requis peut avoir besoin l’application pour terminer la demande présentée par l’utilisateur, telles qu’avoir assez d’argent dans la banque pour effectuer le paiement demandé.

L’application fournit une **intention de réponse** à partir de l’étape de confirmation, qui doit être rempli avec les informations de l’application a disponible pour Siri permettre communiquer efficacement avec l’utilisateur.

Selon le type d’Action et de domaine, Siri peut invite l’utilisateur à confirmer l’opération, telles qu’avant l’envoi d’un paiement ou d’un cas de réservation.

### <a name="the-handle-stage"></a>L’étape de Handle

L’étape gérer est la partie la plus importante de l’utilisation avec l’intention, car il s’agit du point où l’application répond à la demande d’utilisateur en effectuant la tâche qu'a été demandé à faire.

Comme dans l’étape de confirmation, l’application doit fournir plus d’informations sur le résultat que possible afin de Siri pouvez associé à l’utilisateur. Parfois, ces informations seront présentées visuellement ou d’autres fois Siri parle simplement il à l’utilisateur. 

Il peut arriver lorsque l’application peut nécessiter du temps supplémentaire pour traiter une demande donnée, tels que des délais d’appel de réseau, ou si une personne dynamique doit répondre à la demande (comme à la fin et de livraison d’une commande ou gèrent une voiture à l’emplacement de l’utilisateur). Lorsque Siri attend une réponse de l’application, il affiche une interface utilisateur en attente à l’utilisateur indiquant que l’application traite la demande.

Dans l’idéal, l’application doit fournir une réponse à Siri dans les deux à trois secondes au maximum. Si l’application sait qu’une réponse donnée va prendre plus de temps au processus, il doit envoyer un **InProgress** code de réponse à Siri. Siri puis informe l’utilisateur que l’application traite la requête en arrière-plan et continue à le faire de même s’ils quittent l’environnement Siri.

## <a name="adding-sirikit-to-the-app"></a>Ajout de SiriKit à l’application

Avec SiriKit dans iOS 10, Apple a créé deux nouveaux points d’Extension :

- **Extension des intentions** - fournit Siri avec le contenu de l’application et effectue les tâches requises pour répondre à toutes les modes pris en charge.
- **Extension de l’interface utilisateur intentions** -fournit une interface utilisateur personnalisée qui s’affichera pour le contenu des applications à l’intérieur de Siri. 

Il existe également une API pour fournir des mots et expressions à Siri à l’aide de reconnaissance sous la forme :

- **Application vocabulaire** -mots et expressions qui sont communes à tous les utilisateurs de l’application.
- **Utilisateur vocabulaire** -mots et expressions qui sont spécifiques à un utilisateur d’application donnée.

## <a name="the-intents-extension"></a>L’Extension de modes

L’Extension intentions est responsable de la gestion des interactions entre l’application et les Siri principales comme suit :

[![](understanding-sirikit-images/intents01.png "L’Extension de modes")](understanding-sirikit-images/intents01.png#lightbox)

L’intention de l’Extension peut prendre en charge un ou plusieurs objectifs, il incombe au développeur de déterminer la manière dont ils souhaitent mettre en œuvre SiriKit dans l’application. Le développeur peut également ajouter une intention qu’Extension distincte pour chaque objectif devant être gérées.  Ceci dit, Apple demande que le développeur de limiter le nombre d’Extensions de l’intention afin que plusieurs processus n’est ouverte par rapport à l’application, qui nécessitent plus de mémoire et temps à gérer pour Siri.

Le développeur doit également être conscient que l’intention de l’Extension est exécutée en arrière-plan pendant que Siri est active. Cela permet de Siri à exécuter activement sur une conversation avec l’utilisateur lors de la communication toujours avec l’extension pour traiter les informations sur la demande.

## <a name="privacy-and-security-considerations"></a>Considérations de sécurité et confidentialité

Apple a pris des mesures excellentes pour s’assurer qu’un utilisateur privé informations sont sécurisées lors de l’utilisation de Siri et par conséquent, il y a plusieurs interactions qui obligent l’utilisateur à ouvrir une session l’appareil iOS. Par exemple, lors de la demande d’un cas ou d’effectuer un paiement.

En outre, il existe des comportements spécifiques à l’application peut souhaiter de limiter à l’utilisateur qui est connecté à l’appareil. Pour ces situations, l’application peut demander le **restreindre lors du verrouillage** comportement. Cette opération est effectuée via un paramètre dans le `Info.plist` fichier.

L’infrastructure d’authentification Local étant disponible pour l’Extension de l’intention de l’application peut demander à l’utilisateur pour les informations d’authentification supplémentaires, même si l’appareil est déjà déverrouillé.

Enfin, Apple Pay étant disponible pour l’Extension de l’intention de l’application peut effectuer une transaction à l’aide d’Apple Pay et la feuille Apple Pay intégrée s’affiche au-dessus de l’interface Siri.

En outre, les Apple veut s’assurer que les utilisateurs sait quand ils envoient des informations à une application de partie 3e et par conséquent, l’utilisateur **doit** dire le nom spécifique de l’application (comme spécifié dans le nom d’affichage de l’application offre groupée) lorsque vous élaborez une demande.

Apple a conçu Siri à mener à bien des conversations naturelles, fluide avec l’utilisateur et pour cette raison, le nom de groupe de l’application peut être utilisé dans nombreuses parties du discours, partout où elles s’ajustent à naturellement dans la demande de l’utilisateur.

Une des opérations courantes que les utilisateurs seront effectuer consiste à « verbify » le nom de l’application, en d’autres termes, en acceptant le nom de l’application et l’utiliser comme un verbe dans une demande. Par exemple, *« MonkeyChat Bobo ces étaient banane excellente. »*

## <a name="the-intents-ui-extension"></a>L’Extension de l’interface utilisateur de modes

L’Extension de l’interface utilisateur intentions présente la possibilité d’afficher l’interface utilisateur de l’application et les personnalisations dans l’expérience de Siri et rendre les utilisateurs sentir connecté à l’application. Avec cette extension, l’application peut mettre la marque, ainsi que visual et d’autres informations dans la transcription.

[![](understanding-sirikit-images/intents02.png "Exemple de sortie intentions Extension d’interface utilisateur")](understanding-sirikit-images/intents02.png#lightbox)

L’Extension de l’interface utilisateur intentions retourne toujours un `UIViewController` et l’application peut ajouter quoi que ce soit nécessaire dans le contrôleur de vue telles que de l’affichage des informations supplémentaires qui va au-delà de la réponse initiale. L’interface utilisateur intentions peut également mettre à jour l’utilisateur avec l’état d’un événement en cours d’exécution longue, telles que combien de temps prendra une porté voiture pour accéder à leur emplacement de partage.

L’Extension de l’interface utilisateur intentions seront toujours affichée, ainsi que tout autre contenu Siri telles que l’icône de l’application et le nom en haut de l’interface utilisateur ou, selon l’intention, les boutons (par exemple, envoyer ou annuler) peut être affiché en bas.

Il existe quelques cas où l’application peut remplacer les informations qui Siri est affiché à l’utilisateur par défaut tels que la messagerie ou mappe où l’application peut remplacer l’expérience par défaut avec un adaptés à l’application.

> [!IMPORTANT]
> **Remarque :** alors qu’il est possible d’ajouter des éléments interactifs comme `UIButtons` ou `UITextFields` à l’Extension de l’interface utilisateur d’intention `UIViewController`, ceux-ci sont strictement interdites d’intention de l’interface utilisateur dans non interactive et l’utilisateur ne sera pas en mesure d’interagir avec eux.

Il est totalement facultatif pour l’application de fournir une Extension de l’interface utilisateur intention comme Siri contient un ensemble par défaut de l’interface utilisateur pour chaque type d’intention. En outre, les interfaces de l’interface utilisateur des objectifs ne sont disponibles pour certains objectifs que Apple a jugé serait utiles de l’utilisateur.

## <a name="adding-sirikit-vocabulary"></a>Ajout de SiriKit vocabulaire

La dernière partie de l’implémentation SiriKit se trouve dans l’application en fournissant le vocabulaire requis. De nombreuses applications ont des méthodes unique qui décrit les informations à l’utilisateur et que l’utilisateur fournit des informations à l’application de manière unique.

Pour cette raison, Siri requiert une assistance de l’application pour comprendre les termes et expressions uniques à l’application. Certaines de ces expressions feront partie de l’application afin que chaque utilisateur connaître et comprendre les. D’autres sera encore uniques à un utilisateur donné de l’application.

### <a name="app-specific-vocabulary"></a>Spécifique aux applications vocabulaire

Le vocabulaire spécifique d’application définit les mots et expressions spécifiques qui seront connues à tous les utilisateurs de l’application, telles que les types de véhicules ou noms d’entraînement. Voici la partie de l’application, elles sont définies dans un `AppIntentVocabulary.plist` fichier en tant que partie du regroupement principal de l’application. En outre, ces mots et expressions doivent être localisées.

Il existe plusieurs parties à un vocabulaire `AppIntentVocabulary.plist` fichier :

- **Application d’exemple utilise** -ils fournissent un ensemble de cas d’utilisation courants pour les demandes de l’utilisateur peut faire de l’application. Par exemple : *« Démarrer un entraînement avec MonkeyFit ».*
- **Paramètres** -ils fournissent un ensemble de types de paramètres non standard spécifiques à l’application. Par exemple, les noms d’entraînement pour l’application MonkeyFit. Il s’agit de :
    - **Une expression** -permet à l’application de définir des termes uniques pour l’application. Par exemple : le type d’entraînement « Bananarific » pour l’application MonkeyFit. 
    - **Prononciation** -donne des indications de prononciation à Siri comme un simple phonétique une expression donnée. Par exemple, « ba nana ri fic ».
    - **Exemple** -fournit un exemple d’utilisation de l’expression donnée dans l’application. Par exemple, *« Démarrer un Bananarific dans MonkeyFit »*.

Pour plus d’informations, consultez le site d’Apple [référence du Format de fichier de vocabulaire application](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CustomVocabularyKeys.html#//apple_ref/doc/uid/TP40016875-CH10-SW1).

### <a name="user-specific-vocabulary"></a>Vocabulaire spécifique d’utilisateur

Le vocabulaire spécifique utilisateur va fournir des mots ou expressions qui sont uniques à des utilisateurs individuels de l’application. Ces éléments seront fournis lors de l’exécution à partir de l’application principale (pas les Extensions d’application) comme un ensemble ordonné de termes classés dans une priorité d’utilisation plus importante pour les utilisateurs, les termes du contrat plus important au début de la liste.

Examinons l’exemple de l’application MonkeyChat présentée ci-dessus. MonkeyChat conserve une liste de tous les contacts de l’utilisateur, qui envoie à Siri via le vocabulaire spécifique d’utilisateur. Elle conserve également une liste des 10 dernières contacts que l’utilisateur a sollicités et dispose d’un ensemble de contacts favori pour chaque utilisateur. Pour cet exemple, les contacts favoris doivent être au début de notre vocabulaire spécifique d’utilisateur, suivie de contacts récents puis le reste des contacts de l’utilisateur.

Les types d’informations suivants sont pris en charge par le vocabulaire spécifique d’utilisateur :

- Noms de contact.
- Noms d’entraînement.
- Albums photo.
- Mots clés de photo.

Si l’application s’appuie sur le carnet d’adresses d’iOS, l’application n’aura pas d’agir comme ces informations sont déjà disponibles pour Siri. L’application ne doit fournir des noms de contact si l’application a sa propre base de données unique des contacts.

Lorsque vous concevez le vocabulaire, fournir uniquement les valeurs nécessaires que les utilisateurs connaissez et intéressent. Évitez de fournir des informations telles que les numéros de téléphone ou les adresses de messagerie.

L’application doit également mettre à jour rapidement Siri lorsque change de vocabulaire spécifique d’utilisateur. Les utilisateurs sont habitués à demander des informations à partir de Siri l’instant, il a été ajouté à son appareil iOS. Par exemple, si l’utilisateur ajoute un nouveau contact dans l’application, envoyer ces informations à Siri dès que l’utilisateur enregistre.

Plus important encore, l’application _doit_ supprimer rapidement des informations à partir du vocabulaire Siri dans la mesure où un utilisateur peut devenir perturber si elles supprimé un élément d’information mais Siri a été le reconnaître en heures ou jours plus tard.

> [!IMPORTANT]
> **Remarque :** l’application doit supprimer le vocabulaire spécifique utilisateur de Siri si l’utilisateur choisit de réinitialiser l’application ou si leur déconnexion.

## <a name="sirikit-permissions"></a>Autorisations SiriKit

La dernière partie du SiriKit est centrée sur les autorisations. Tout comme l’utilisation d’autres fonctionnalités d’e/s (telles que les Photos ou une caméra Contacts), les utilisateurs doivent accorder l’autorisation explicite pour l’application de communiquer avec Siri.

L’application est en mesure de fournir une chaîne qui définit les informations qu’il sera à Siri et proposent une raison que pour la raison pour laquelle l’utilisateur doit accorder cet accès. 

Apple suggère que l’application doit demander l’autorisation de l’utilisateur à utiliser lors de Siri le premier que l’utilisateur ouvre l’application une fois qu’ils ont mis à niveau vers iOS 10. Il s’agit pour que les utilisateurs savoir sur l’intégration de Siri et qu’il peuvent utilisation approbation préalable avant de procéder à sa première demande.

## <a name="sirikit-and-maps"></a>SiriKit et tables

SiriKit fait partie intégrante d’e/s et utilise une infrastructure plus grande intentions ajoutée à iOS 10. Le framework intentions a été conçu pour partager des actions courantes et partagées et des intentions avec d’autres parties du système.

Le framework intentions va au-delà simplement Siri intégration et fournit d’autres fonctionnalités telles que l’intégration de Contacts où l’application peut devenir la téléphonie de la valeur par défaut ou une application de messagerie des contacts spécifiques. Intentions fournissent également l’intégration en profondeur avec CallKit pour fournir aux utilisateurs la meilleure expérience VOIP possible.

L’application de mappages dans iOS 10 a ajouté des fonctionnalités telles que le partage de cas où l’utilisateur peut réserver une porté directement à l’intérieur de l’interface utilisateur de cartes. SiriKit fournit un point d’extension commun avec des cartes pour les modes de partage (et autres) porté peuvent être partagées entre Siri et des mappages. 

Cela signifie que si l’application a arrêté les extensions SiriKit, il permet également d’établir l’intégration de cartes. 

## <a name="designing-a-great-siri-experience"></a>Conception d’une grande expérience Siri

Conception d’une expérience utilisateur satisfaisante lors de l’intégration d’une application dans Siri est différente de la conception d’une interface utilisateur d’application réussie. À la différence des circonstances normales, où l’utilisateur interagit avec l’application directement suite écran, lors de l’utilisation de siri quand il sont souvent lorsque aucune interface visuelle n’est visible à tous. Par exemple, lorsque l’utilisateur a démarré la conversation avec *« Hey Siri »*.

### <a name="how-siri-helps-the-developer"></a>Comment Siri permet au développeur

Lorsque vous concevez les interactions d’une application avec Siri, vous devez créer l’application un *Interface conversation*, ce qui signifie que le contexte est dérivé de la conversation dont Siri est à l’utilisateur sur le nom de l’application.

En l’absence d’une référence visuelle, l’utilisateur doit suivre des informations présentées dans leur en-tête. Pour cette raison, Siri présente les informations minimales requises pour obtenir la tâche de l’utilisateur souhaitant de faire.

L’Interface de conversation est mise en forme par les questions et réponses de l’utilisateur et le Siri au cours de la conversation. Il est donc important de réfléchir à la façon dont Siri pose des questions et répond lors de la conception de cette interface.

Prenons l’exemple suivant de l’utilisateur qui crée un message, Siri peut répondre à la question *« Prêt à l’envoyer ? »* . L’utilisateur peut répondre de différentes façons, telles que *« Envoyer »*, *« Annuler »* ou même une chose totalement non liés à cette question. Quelle que soit la façon dont la conversation se déroule, Siri gérer pour l’application et uniquement envoyer les informations pertinentes dès que possible.

Il existe plusieurs façons différentes, que l’utilisateur peut initier une conversation avec Siri :

- En reprenant l’appareil, en appuyant sur le bouton Accueil. Dans ce cas Siri présente plusieurs interfaces visual et moins réponses verbales.
- En disant *« Hey Siri »* et démarrage d’une conversation mains libres. Dans ce cas Siri sera moins visual et plus verbale.
- À l’aide des fonctionnalités d’accessibilité tels que bluetooth activé auditifs où l’interface utilisateur personnalisé en fonction d’un utilisateur ayant des besoins particuliers.
- À l’aide de la lecture de voiture où l’utilisateur a besoin de conserver leur attention le focus sur la conduite en conservant les éléments indésirables au minimum.

### <a name="how-the-developer-helps-siri"></a>Comment le développeur peut aider à Siri

Lors de l’intégration d’une application avec Siri, le développeur doit souvent cette intégration de test et vous assurer qu’effectuer plusieurs demandes différentes en posant pour la même partie de la tâche ou des informations différemment autant que possible.

Dans la mesure où aucun deux ne personnes de réflexion similaire, il est essentiel que le développeur Obtient des testeurs différents bêta autant que possible pour aider à ajuster l’intégration Siri. Les utilisateurs peuvent demander des informations ou vérifiez les requêtes de façons le développeur jamais cependant d’et ce réglage peut aider à vérifier que le groupe plus large d’utilisateurs ont une expérience optimale à l’aide de leur application avec Siri.

Dans des situations différentes et environnements de test. Initier les conversations avec Siri dans toutes les méthodes possibles garantir que ces conversations demeurent fluide et naturelles. Tester dans les emplacements où l’utilisateur plus probablement utilisent l’application, comme dans un gym encombré.

Assurez-vous que l’application fournit toutes les informations que Siri doit représenter correctement la demande et le résultat à l’utilisateur. Cela est particulièrement vrai lors de l’utilisation de Siri dans une situation mains libres.

### <a name="siri-design-guidelines"></a>Règles de conception de Siri

N’oubliez pas que Siri rencontre une conversation avec l’utilisateur de la part de l’application. Le développeur souhaite ignorez les conséquences que cette conversation reste aussi fluide et naturel que possible.

Comme ils le feraient avec n’importe quel conversation importante, le développeur doit s’assurer que les éléments suivants :

- Que l’application est prête pour la conversation.
- Que l’application écoute exactement ce que l’utilisateur essaie de faire.
- Que l’application pose les questions appropriées aux moments appropriés.
- Que l’application répond à la demande avec les informations de l’utilisateur est la recherche.

#### <a name="preparing-for-the-conversation"></a>Préparation de la Conversation

La première chose à retenir est que les utilisateurs de l’application ne vont pas être exactement comme le développeur. Ils peuvent être de différents arrière-plans, parlent différentes langues ou avez des besoins spéciaux lorsque vous travaillez avec l’application.

En outre, étant donné que le développeur conçu et créé de l’application, ils ont profondes, une connaissance approfondie de l’application et son fonctionnement interne et les fonctions qui ne disposera d’un utilisateur. Par conséquent, le développeur peut demander demande de Siri différemment d’un utilisateur normal.

C’est pourquoi il est essentiel d’avoir que plusieurs personnes possible interagissent avec l’application via Siri. Les utilisateurs peuvent effectuer des demandes de l’application via Siri le développeur n’a jamais considéré d’ou comme le développeur n’a pas prendre en compte.

#### <a name="ensure-the-app-is-a-good-listener"></a>Vérifiez que l’application est un bon port d’écoute

Le développeur doit s’assurer que l’application est un écouteur de bon et reçoit les spécificités de la conversation qui répondent aux attentes de l’utilisateur. Mais il est également possible qu’ils n’avez pas fourni toutes les informations que l’application a besoin pour effectuer la tâche demandée.

Il existe plusieurs méthodes que l’application peut gérer cette situation :

- **Choisir la bonne valeur par défaut pour la valeur manquante** , par exemple une application de partage de gestion peuvent être par défaut à l’emplacement actuel de l’utilisateur si elles ne spécifiait pas où ils souhaitent être récupéré à partir de.
- **Obtenues** -à l’aide des informations spécifiques que l’application a collectées sur l’utilisateur, l’application peut être en mesure de marque et obtenues sur les informations manquantes, telles que le remplissage d’un numéro de téléphone portable manquant à partir des informations de contact de l’utilisateur. Il convient toutefois, évitez les surprises incorrects, tels que l’option la plus chère, etc. de prélèvement.
- **Invite de commandes pour plus d’informations** -l’application peut avoir Siri inviter l’utilisateur à la valeur manquante. Toutefois, la clé ici consiste à maintenir les conversations simple et pour le point. Les utilisateurs deviendront rapidement déçus si elles ont pour répondre à plusieurs questions pour atteindre leur demande.
- **Gérer naturellement désinformation** -l’utilisateur peut fournir une valeur qui n’était pas attendu. l’application ou qu’il ne peut pas gérer dans le contexte donné. Assurez-vous que l’application est liée à cette situation à l’utilisateur d’une manière qui rend claire et facile pour les corriger.

Lorsque l’application est présentée avec une valeur unique qui est en question, la meilleure façon de gérer cette situation consiste à avoir Siri à demander à l’utilisateur à confirmer l’opération. Par exemple, *« Vouliez-vous Bobo le grand ? »* , lequel ils peuvent répondre à une simple réponse Oui ou non.

Lorsqu’il existe une situation dans laquelle plusieurs choix possibles peut être adaptés à une valeur unique, ambiguïté est la méthode de gestion par défaut. Dans ce cas les Siri peut inviter l’utilisateur avec jusqu'à dix possibles d’options. Exemple :

```csharp
Who do you want to send the message to?

* Bobo the Great!
* Bobo Jr.
* Little Bobo
```

Toujours en question, avoir Siri inviter l’utilisateur à fournir une réponse totalement nouvelle, plus spécifique pour une valeur donnée.

#### <a name="request-final-confirmation"></a>Demander la Confirmation finale

Avant que l’application effectue la tâche afin de répondre à la demande de l’utilisateur, Siri vérifiera avec l’Extension d’application pour s’assurer que tout est en place. Par exemple, l’utilisateur n’a pas assez d’argent dans leur compte pour effectuer le paiement demandé ?

En outre, l’application doit s’assurer qu’il fournit toutes les informations possibles à Siri afin de présenter à l’utilisateur et vérifiez que la tâche à exécuter répond à leurs attentes.

Une fois que l’utilisateur a confirmé la demande et l’application a effectuée, l’application doit à nouveau vous assurer qu’il a fourni tous les résultats à Siri afin qu’il peut être associée à l’utilisateur.

#### <a name="responding-to-the-request"></a>Répondre à la demande

Siri comporte plusieurs Interfaces utilisateur intégré pour chacun des domaines et des Actions qu’il connaît sur. Toutefois, le cas échéant, l’application peut fournir une Extension de l’interface utilisateur personnalisée intention pour enrichir l’expérience utilisateur en présentant le logo de l’application et l’interface utilisateur ou plus d’informations que n’était présent dans la demande.

Ceci dit, retenue doit être utilisé lors de la conception des interfaces personnalisées pour Siri. En règle générale, l’utilisateur souhaitant d’obtenir une tâche spécifique faite aussi rapidement que possible et ne souhaitez pas être surchargé avec des informations inutiles.

Il convient également pour vous assurer que l’interface utilisateur personnalisée recherche et répond correctement dans tous les appareils iOS différents et les orientations que l’utilisateur peut avoir ou être à l’aide de l’appareil.

Lorsque cela est approprié, utilisez l’API SiriKit pour masquer toutes les informations redondantes déjà présentes dans la valeur par défaut Siri UI. Encore vous assurer que l’application est toujours en fournissant les informations à Siri afin qu’il peut le présenter verbalement dans un cas mains libres.

Il peut y avoir des situations où Siri lance l’application pour répondre à la demande de l’utilisateur, telles que présentant les photos de que l’utilisateur a demandé. Dans ces situations, ne surpris par l’utilisateur. Afficher les informations attendues sans avoir les étapes intermédiaires ou davantage d’interaction. Ne jamais afficher des informations ou effectuer une tâche de que l’utilisateur n’est pas attendu.

### <a name="polishing-the-design"></a>Finition de la conception

Il existe plusieurs étapes Apple vous suggérons de peaufiner la conception des Interfaces de conversation. Est tout d’abord, en fournissant claire et concises, vocabulaire et utilisez cas exemples pour Siri.

Une des façons qu’un utilisateur détecte l’application est en lançant une conversation avec Siri et de la demande, *« Que pouvez-vous faire ? »* Siri affiche plusieurs choses il peut le faire, y compris les applications de développeur qui le héros exemple des scénarios qui il a fourni via son `plist` fichier.

Comment écrire des cas d’usage bon exemple :

- Assurez-vous que les exemples incluent le nom de l’application.
- Gardez à l’exemple à court et à point.
- Fournit plusieurs exemples pour chacun des intentions de l’application prend en charge.
- Hiérarchiser les intentions et les exemples dans les selon les cas d’usage courants pour l’application.
- Vérifiez que l’application fournit des exemples localisés.
- Vérifiez que chaque exemple fonctionne comme prévu dans l’application.
- Éviter d’adressage dans les exemples, afin de ne pas inclure de texte comme Siri *« … de Siri Hey »*
- Éviter de tels que n’importe quel pleasantries inutiles *«, »* ou *« Merci »*.

Prenez le temps approprié pour Explorer et faire des essais sur la façon dont l’application en forme la conversation dont Siri est à l’utilisateur en son nom. Assurez-vous de discuter avec les utilisateurs habituels tout au long du processus, en tant que leurs interactions avec et aux attentes de l’application peuvent changer au fil du temps.

N’oubliez pas de tester l’application dans différentes situations et toutes les différentes méthodes pour appeler une conversation avec Siri. Test dans des emplacements du monde réel de l’utilisateur peut être à l’aide de l’application, en s’éloignant de l’office et le support.

S’efforcent devra être fluide, naturel et « sentiront simplement » les conversations avec Siri (pour le compte de l’application). 

## <a name="summary"></a>Récapitulatif

Cet article a couvert les concepts clés requis pour utiliser SiriKit et indiqué qu’il peut interagir avec les applications Xamarin.iOS pour fournir des services qui sont accessibles à l’utilisateur à l’aide de Siri et l’application de cartes sur un appareil iOS.




## <a name="related-links"></a>Liens associés

- [Exemple de ElizaChat](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [Guide de programmation SiriKit](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [Référence de Framework intentions](https://developer.apple.com/reference/intents)
- [Référence de Framework intentions l’interface utilisateur](https://developer.apple.com/reference/intentsui)
