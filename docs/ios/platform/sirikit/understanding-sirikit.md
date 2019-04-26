---
title: Présentation des concepts de SiriKit
description: Ce document décrit les concepts clés nécessaires pour travailler avec SiriKit dans une application Xamarin.iOS. Par exemple, il aborde les intentions et la conception d’extensions d’IU Intents, autorisations de SiriKit, une expérience optimale et bien plus encore.
ms.prod: xamarin
ms.assetid: 99EC5C1E-484F-4371-8555-58C9F60DE37F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/02/2017
ms.openlocfilehash: b2a9e757e8a3407bbb19ae0580e5788eabe84cf0
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61188921"
---
# <a name="understanding-sirikit-concepts"></a>Présentation des concepts de SiriKit

_Cet article aborde les concepts fondamentaux qui seront nécessaires pour l’utilisation de SiriKit dans une application Xamarin.iOS._


Nouveau à iOS 10, SiriKit permet à une application Xamarin.iOS fournir des services qui sont accessibles à l’utilisateur à l’aide de Siri et l’application Maps sur un appareil iOS. Cette fonctionnalité est fournie dans un ou plusieurs Extension d’application à l’aide de la nouvelle **intentions** et **Intents UI** frameworks.

SiriKit permet à une application iOS fournir des services qui sont accessibles à l’utilisateur à l’aide de Siri et l’application Maps sur un appareil iOS à l’aide des Extensions d’application et le nouveau **intentions** et **Intents UI** frameworks.

Siri fonctionne avec le concept de **domaines**, groupes de connaître les actions pour les tâches associées. Chaque interaction de l’application avec Siri doit appartenir à l’un de ses domaines de service connues comme suit :

- Audio ou vidéo.
- Un tour de réservation.
- La gestion de sommeil.
- La messagerie.
- Recherche des photos.
- Envoyer ou recevoir des paiements.

Lorsque l’utilisateur effectue une requête de Siri impliquant un des services de l’Extension application, SiriKit envoie l’extension un **intention** objet qui décrit la demande de l’utilisateur, ainsi que les données pertinentes. l’Extension d’application génère ensuite approprié **réponse** de l’objet pour la donnée **intention**, détaillant la façon dont l’extension puisse traiter la demande.

## <a name="the-intents-and-intents-ui-extensions"></a>Les intentions et les Extensions de l’interface utilisateur Intents

Siri et l’application Maps interagissent avec les services de l’application par le biais de deux types d’Extensions d’application :

- **Extension Intents** -cartes avec l’application Siri fournit du contenu et effectue les tâches nécessaires pour remplir les intentions pris en charge.
- **Extension d’IU Intents** -fournit une interface utilisateur personnalisée qui s’affichera pour le contenu de l’application à l’intérieur de Siri ou plans.

L’application doit fournir une Extension Intents pour prendre en charge de SiriKit et il est chargé pour fournir des informations qui Siri et Maps peuvent présenter à l’utilisateur et la gestion des intentions.

Création d’une Extension d’IU Intents est facultative dans la mesure où Siri généralement gère toutes les interactions utilisateur a une interface utilisateur intégrée, standard, pour présenter des informations dans chacun des domaines pris en charge. En fournissant une Extension d’IU Intents, l’application peut utiliser le **l’interface utilisateur de l’intention** framework pour présenter une riche et personnalisé l’Interface utilisateur présentant la marque de l’application et des informations supplémentaires.

## <a name="siri-and-the-maps-app-role"></a>Siri et le rôle d’application cartes

Requêtes parlé de l’utilisateur sont traité et analysé sémantiquement par Siri qui transforme ces demandes en intentions exploitables capable de gérer les Extensions de l’intention de langage.

Maps utilise les Extensions d’intention de l’application pour afficher des informations dans l’interface de carte en réponse aux actions de l’utilisateur. Telles que la demande à proximité de restaurants ou bien le restaurant de l’application passe en revue.

Siri et cartes de gérer toutes les interactions de l’utilisateur et affichent les résultats à l’aide de l’interface du système standard. Le rôle d’application extensions consiste principalement à fournir les données qui s’affiche. Si vous le souhaitez, l’application peut fournir une Extension d’IU Intents et présenter une interface utilisateur personnalisée pour des raisons de l’interface du système par défaut.

## <a name="interacting-with-siri-via-sirikit"></a>Interaction avec Siri par le biais de SiriKit

Cette section présente une vue d’ensemble de la façon dont SiriKit permet à l’utilisateur d’interagir avec l’application à l’aide de Siri. Cet exemple, nous allons utiliser l’application MonkeyChat fausse :

[![](understanding-sirikit-images/monkeychat01.png "L’icône MonkeyChat")](understanding-sirikit-images/monkeychat01.png#lightbox)

MonkeyChat conserve son propre contact livre d’amis de l’utilisateur, chacun étant associé à un nom de l’écran (par exemple, Bobo par exemple) et permet à l’utilisateur d’envoyer des conversations de texte à chaque ami par leur nom de l’écran.

Il existe de nombreuses façons que l’utilisateur peut initier une interaction avec l’application, dans la mesure où différentes personnes peuvent être la même demande sous différentes formes.

Par exemple, si l’utilisateur souhaite envoyer un message à leur friend Bobo, ils devront peut-être la conversation suivante avec Siri :

_Utilisateur : Bonjour Siri, envoyer un message MonkeyChat._<br />
_Siri : Auxquels ?_<br />
_Utilisateur : Bobo._<br />
_Siri : Que voulez-vous dire à Bobo ?_<br />
_Utilisateur : Veuillez envoyer bananes plus._<br />

Une autre personne peut être la même requête avec une autre conversation :

_Utilisateur : Envoyer un message à Bobo sur MonkeyChat._<br />
_Siri : Que voulez-vous dire à Bobo ?_<br />
_Utilisateur : Veuillez envoyer bananes plus._<br />

Et un autre utilisateur peut effectuer une demande encore plus courte :

_Utilisateur : MonkeyChat Bobo envoyez bananes plus._<br />
_Siri : OK, ssage envoyez bananes plus à Bobo sur Monkeychat._<br />

Ou même faire la demande même dans une autre langue :

_Utilisateur : MonkeyChat Bobo Veuillez vous plaît envoyer plus de bananes._<br />
_Siri : Oui, envoyer d’envoi message Veuillez vous plaît ainsi que de bananes à Bobo sur Monkeychat._<br />

Encore un autre utilisateur peut être très détaillé dans leur conversation :

_Utilisateur : Bonjour Siri, pouvez-vous Veuillez me faire plaisir et lancez l’application MonkeyChat pour envoyer un SMS avec le message, envoyez bananes plus._<br />
_Siri : Auxquels ?_<br />
_Utilisateur : Mon pal meilleures Bobo._<br />

En outre, il existe de nombreuses façons Siri peut répondre à une requête, certaines selon la façon dont la demande a été effectuée :

- **En maintenant le bouton accueil** -Siri fournira plus de réponses visual avec commentaires verbaux limitée.
- **Par « Hey Siri »** -Siri sera plus verbale et fournir des réponses visual moins.

Siri est également réglée pour répondre aux besoins d’accessibilité de l’utilisateur sera interagir et répondre en fonction de ces besoins.

Quelle que soit la façon dont une demande est faite, ou comment Siri répond à la demande, Siri gère la conversation avec l’utilisateur et l’application (par le biais de ses Extensions) fournit les fonctionnalités.

Lorsque l’utilisateur effectue une requête verbale de Siri, voici les étapes qui suivre Siri :

[![](understanding-sirikit-images/monkeychat02.png "Les étapes qui suivre Siri")](understanding-sirikit-images/monkeychat02.png#lightbox)

1. Tout d’abord, Siri prend le contenu audio de l’utilisateur **vocale** et le convertit en texte.
2. Ensuite, le texte est converti en un **intention**, un structuré de représentation sous forme de demande de l’utilisateur.
3. Selon l’intention, Siri prendra **Action** pour effectuer la demande de l’utilisateur.
4. Enfin, présentera Siri **réponses** (visual et verbale) à l’utilisateur en fonction de l’Action effectuée.

Il existe trois méthodes principales que l’application peut participer à la conversation de l’utilisateur avec Siri :

[![](understanding-sirikit-images/monkeychat03.png "Les trois méthodes principales que l’application peut prendre part à la conversation d’utilisateurs avec Siri")](understanding-sirikit-images/monkeychat03.png#lightbox)

1. **Vocabulaire** -Voici comment l’application indique à Siri les mots qu’il a besoin pour interagir avec lui.
2. **Logique d’application** : ce sont les actions et selon les réponses que l’application prendra donné intentions.
3. **Interface utilisateur** -c’est l’interface utilisateur personnalisée facultative que l’application peut donner ses réponses dans.

### <a name="example"></a>Exemple

Étant donné les informations ci-dessus, examinez l’interaction de la conversation suivante avec l’application MonkeyChat :

_Utilisateur : Bonjour Siri, envoyer un message à Bobo sur MonkeyChat._<br />
_Siri : Que voulez-vous dire à Bobo ?_<br />
_Utilisateur : Veuillez envoyer bananes plus._<br />

Le premier rôle de l’application prend à la conversation est d’aider les Siri à comprendre les voix de l’utilisateur :

[![](understanding-sirikit-images/monkeychat04.png "Aider Siri à comprendre les paroles d’utilisateurs")](understanding-sirikit-images/monkeychat04.png#lightbox)

Siri n’a pas le nom « Bobo » dans sa base de données, mais l’application effectue et a partagé ces informations avec Siri via son vocabulaire. L’application permet également de Siri reconnaître que Bobo est un destinataire, dans la mesure où il les spécifié à Siri comme un *Contact*.

Siri sait que plus est nécessaire pour envoyer un message à simplement un destinataire, donc il vérifiera rapidement avec l’Extension d’application pour voir si un message nécessite de contenu. Dans la mesure où MonkeyChat est le cas, Siri répondra à l’utilisateur avec la question : *« Ce que voulez-vous dire à Bobo ? »*

Dans l’exemple ci-dessus, l’utilisateur a répondu, *« Envoyez bananes plus »*, qui regroupe dans un texte structuré Siri **intention**:

[![](understanding-sirikit-images/monkeychat05.png "Siri regroupe réponse de l’utilisateur dans une intention structurée")](understanding-sirikit-images/monkeychat05.png#lightbox)

L’objectif structurée contiendra les informations suivantes :

- **Domaine :** Messages
- **Objectif :** sendMessage
- **Destinataire :** Bobo
- **Contenu :** Veuillez envoyer plus bananes

Chaque domaine a en tant qu’ensemble de savoir *Actions* qui peut être effectuée en leur sein et basé sur le domaine et l’Action, de nombreux paramètres peuvent être inclus dans le but de zéro est envoyée à l’application.

L’objectif est ensuite envoyé à l’Extension d’application pour traitement. Par conséquent l’application de traitement de l’intention, génère un **IntentResponse** qui est livré avec l’intention et inclure des paramètres qui décrivent ce que l’application se fait avec l’intention.

Chaque IntentResponse inclut également un **Code de réponse** ce qui indique à Siri si l’application a été en mesure d’effectuer la demande ou non. Certains domaines ont des codes de réponse d’erreur très spécifique qui peuvent être envoyés ainsi.

Enfin, le IntentResponse inclura un `NSUserActivity` (telles que celles utilisées pour prendre en charge remette). Le `NSUserActivity` sera utilisé pour lancer l’application si la réponse en a besoin pour laisser l’environnement Siri et entrez l’application pour le terminer. 

Siri génère automatiquement un approprié `NSUserActivity` pour démarrer l’application et les lieux où l’utilisateur laissées dans l’environnement de Siri. Toutefois, l’application peut fournir sa propre `NSUserActivity` avec personnalisé d’informations, si nécessaire.

Une fois que l’application a traité l’intention et a retourné une réponse à Siri, elle présente ensuite les résultats à l’utilisateur (verbalement et visuellement) :

[![](understanding-sirikit-images/monkeychat06.png "Les résultats présentés à l’utilisateur à la fois verbalement et visuellement")](understanding-sirikit-images/monkeychat06.png#lightbox)

Siri a réponse intégrée plusieurs Interfaces utilisateur pour chacun des domaines disponibles pour l’application. Toutefois, étant donné que MonkeyChat a fourni une Extension de l’interface utilisateur intention facultative, il est utilisé pour présenter les résultats de la conversation à l’utilisateur dans l’exemple ci-dessus.

## <a name="the-intent-lifecycle"></a>Le cycle de vie Intent

Il existe trois tâches principales que l’Extension d’application devez effectuer lorsque vous traitez des intentions :

[![](understanding-sirikit-images/monkeychat07.png "Le cycle de vie Intent")](understanding-sirikit-images/monkeychat07.png#lightbox)

1. L’application doit **résoudre** chaque paramètre sur un événement. Par conséquent, l’application appellera résoudre plusieurs fois (une fois par chaque paramètre) et parfois plusieurs fois sur le même paramètre jusqu'à ce que l’application et l’utilisateur d’accord sur ce qui est demandé.
2. L’application doit **confirmer** qu’il peut gérer l’intention demandée et savoir Siri sur le résultat attendu.
3. Enfin, l’application doit **gérer** l’intention et effectuer les étapes permettant d’obtenir le résultat demandé.

### <a name="the-resolve-stage"></a>La phase de résolution

La phase de résolution permet de comprendre les valeurs que l’utilisateur a fourni et s’assure que ce que l’utilisateur signifiait réellement est ce qui se produit lorsque l’intention est traitée par l’application Siri. 

Cette étape fournit également une opportunité pour l’application influencer le comportement de Siri pendant la conversation avec l’utilisateur. Pour ce faire, l’application fournira un **résolution réponse**. Il existe un nombre de réponse prédéfini pour les différents types de données qui comprend de Siri.

La plus courante réponse de résolution à partir de l’application sera **réussite**, ce qui signifie que l’application mise en correspondance avec l’élément de données spécifique à partir d’un paramètre (par exemple, le nom de l’écran utilisateur) une partie des informations qu’il connaît.

Il peut arriver lorsque l’application a besoin pour confirmer qu’une requête donnée correspond à l’information correcte qu’il connaît. Dans ce cas, il enverra un **ConfirmationRequired** réponse à poser une question Oui ou non à l’utilisateur telles que *« Envoyer le message à Bobo le grand ? »*

Il existe peut-être d’autres cas où l’application nécessite que l’utilisateur à choisir parmi une liste courte des options. Dans ce cas, l’application fournira un **DÉSAMBIGUÏSATION** réponse avec une liste d’options de deux à dix pour l’utilisateur de choisir à partir de telles que : 

```csharp
Who do you want to message?

* Bobo the Great
* Bobo Jr.
* Little Bobo
```

Siri gère l’utilisateur qui effectue la sélection, verbalement ou en interagissant avec l’UI Siri, et le résultat sera envoyé à l’application.

Dans d’autres cas, il peut ne pas être suffisamment d’informations pour l’application résoudre le paramètre ou qu’il peut y avoir trop de correspondances à résoudre à l’aide de la levée d’ambiguïté (par exemple, 80 utilisateurs avec Bobo dans leur nom). Dans ce cas, l’application envoie un **NeedsMoreDetails** réponse et Siri invitera l’utilisateur pour être plus précis.

Si l’utilisateur n’a pas fourni une valeur qui est requise pour traiter l’intention, il peut envoyer un **NeedsValue** réponse avoir Siri d’inviter l’utilisateur à la valeur.

Si l’application ne prend pas en charge une valeur que l’utilisateur a donné pour un paramètre spécifique, il peut envoyer le **UnsupportedWithReason** réponse à fournir une raison pourquoi la valeur n’a pas été pris en charge. Siri puis invitera l’utilisateur pour une valeur complètement nouveau et leur la raison pour laquelle il est requis.

Enfin utiliser le **NotRequired** réponse pour indiquer à Siri que l’application ne nécessite pas une valeur pour un paramètre donné. Si l’utilisateur fournit un malgré tout, il sera simplement ignoré par Siri.

### <a name="the-confirm-stage"></a>L’étape de confirmation

L’étape de confirmation a deux objectifs :

- Pour indiquer le résultat attendu de gérer une intention afin que Siri peut indiquer à l’utilisateur ce qui va se produire à Siri.
- Fournit une vérification de l’opportunité d’états requis, l’application peut avoir besoin pour terminer la demande, présentée par l’utilisateur, telles que d’avoir assez d’argent dans la banque pour le paiement demandé.

L’application fournit un **intention réponse** à partir de l’étape de confirmation, qui doit être rempli avec comme quantité d’informations l’application dispose donc Siri peut communiquer efficacement avec l’utilisateur.

Selon le type de domaine et l’Action, Siri peut-être inviter l’utilisateur à confirmer l’opération, comme avant l’envoi d’un paiement ou d’un tour de réservation.

### <a name="the-handle-stage"></a>L’étape de Handle

L’étape gérer est la partie la plus importante de l’utilisation d’une intention, car il s’agit du point où l’application répond à la demande l’utilisateur en effectuant la tâche qu’il a été invité à effectuer.

Comme elle le faisait dans l’étape de confirmation, l’application doit fournir autant d’informations sur le résultat que possible afin de Siri pouvez associé à l’utilisateur. Parfois, ces informations seront présentées visuellement ou d’autres fois Siri parle simplement il à l’utilisateur. 

Il peut arriver lorsque l’application peut nécessiter plus de temps pour traiter une demande donnée, tels que des délais d’appel de réseau ou si une personne en direct doit répondre à la demande (comme à la fin et un bordereau d’expédition ou conduite d’une voiture à l’emplacement de l’utilisateur). Lorsque Siri attend une réponse à partir de l’application, il affichera une interface utilisateur en attente à l’utilisateur lui indiquant que l’application traite la requête.

Dans l’idéal, l’application doit fournir une réponse à Siri dans les deux à trois secondes au maximum. Si l’application sait qu’une réponse particulière va prendre plus de temps au processus, elle doit envoyer un **InProgress** code de réponse à Siri. Siri puis informe l’utilisateur que l’application traite la requête en arrière-plan et continuera à faire de même, s’ils quittent l’environnement de Siri.

## <a name="adding-sirikit-to-the-app"></a>Ajout de SiriKit à l’application

Avec SiriKit dans iOS 10, Apple a créé deux nouveaux points d’Extension :

- **Extension Intents** - fournit Siri avec le contenu de l’application et effectue les tâches nécessaires pour remplir les intentions pris en charge.
- **Extension d’IU Intents** -fournit une interface utilisateur personnalisée qui s’affichera pour le contenu des applications à l’intérieur de Siri. 

Il existe également une API pour fournir des mots et expressions à Siri afin de faciliter la reconnaissance sous la forme de :

- **Application vocabulaire** -mots et expressions qui sont communes à tous les utilisateurs de l’application.
- **Utilisateur vocabulaire** -mots et expressions qui sont uniques à un utilisateur de l’application donnée.

## <a name="the-intents-extension"></a>L’Extension Intents

L’Extension Intents est chargée de gérer les interactions principales entre l’application et le Siri comme suit :

[![](understanding-sirikit-images/intents01.png "L’Extension Intents")](understanding-sirikit-images/intents01.png#lightbox)

L’Extension d’intention peut prendre en charge un ou plusieurs des intentions, il incombe au développeur de déterminer la façon dont ils souhaitent implémenter SiriKit dans l’application. Le développeur peut également ajouter aucune Extension intention distincte pour chaque objectif devant être gérée.  Ceci dit, Apple demande que le développeur limiter le nombre d’Extensions de l’intention afin que plusieurs processus n’est ouvert par rapport à l’application, qui nécessitent davantage de mémoire et temps à gérer sur Siri.

Le développeur doit également être conscient que l’Extension intention s’exécuteront en arrière-plan pendant que Siri est actif. Cela permet de Siri activement exercer une conversation avec l’utilisateur lors de la communication toujours avec l’extension pour traiter les informations sur la demande.

## <a name="privacy-and-security-considerations"></a>Considérations de sécurité et confidentialité

Apple a pris des mesures excellents pour garantit qu’un utilisateur privé informations sont sécurisées lors de l’utilisation de Siri et par conséquent, il existe plusieurs interactions qui oblige l’utilisateur à ouvrir une session l’appareil iOS. Par exemple, lors de la demande d’une course ou d’effectuer un paiement.

En outre, il existe des comportements spécifiques que l’application peut souhaiter de limiter à l’utilisateur qui est connecté à l’appareil. Pour ces situations, l’application peut demander le **restreindre lors du verrouillage** comportement. Cette opération est effectuée via un paramètre dans le `Info.plist` fichier.

L’infrastructure d’authentification Local étant disponible pour l’Extension de l’intention de l’application peut demander à l’utilisateur pour les informations d’authentification supplémentaires, même si l’appareil est déjà déverrouillé.

Enfin, Apple Pay étant disponible pour l’Extension de l’intention de l’application peut effectuer une transaction à l’aide d’Apple Pay et la feuille d’Apple Pay intégrée s’affiche au-dessus de l’interface Siri.

En outre, Apple veut s’assurer que les utilisateurs sait quand ils envoient des informations à une application tierce 3e et par conséquent, l’utilisateur **doit** dire le nom spécifique de l’application (comme spécifié dans le nom d’affichage de l’application offre groupée) lorsque vous élaborez une demande.

Apple a conçu Siri à mener des conversations naturelles et fluides avec l’utilisateur et pour cette raison, nom du Bundle de l’application peut être utilisé dans nombreuses parties du discours, où qu’elles s’ajustent à naturellement dans la demande de l’utilisateur.

Parmi les choses auxquelles les utilisateurs peuvent faire consiste à « verbify » le nom de l’application, en d’autres termes, en acceptant le nom de l’application et l’utiliser comme un verbe dans une requête. Par exemple, *« MonkeyChat Bobo ce fut une excellentes bananes. »*

## <a name="the-intents-ui-extension"></a>L’Extension d’IU Intents

L’Extension d’IU Intents offre la possibilité d’afficher l’interface utilisateur de l’application et de personnalisation dans l’expérience de Siri et rendre les utilisateurs se sentent connectés à l’application. Avec cette extension, l’application peut apporter la marque, ainsi que les visuels et d’autres informations dans la transcription.

[![](understanding-sirikit-images/intents02.png "Exemple de sortie Extension d’IU Intents")](understanding-sirikit-images/intents02.png#lightbox)

L’Extension d’IU Intents retournent toujours un `UIViewController` et l’application peut ajouter quoi que ce soit nécessaire à l’intérieur du contrôleur d’affichage tels qu’affichant des informations supplémentaires qui vont au-delà de la réponse initiale. L’interface utilisateur Intents peut également mettre à jour de l’utilisateur avec l’état d’un événement de long terme, par exemple combien de temps prendra un tour voiture pour atteindre leur emplacement de partage.

L’Extension d’IU Intents seront toujours affichée, ainsi que d’autres contenus Siri telles que l’icône de l’application et le nom en haut de l’interface utilisateur ou, selon l’intention, des boutons (par exemple, envoyer ou annuler) peut être affiché en bas.

Il existe quelques cas où l’application peut remplacer les informations qui Siri affiche à l’utilisateur par défaut comme la messagerie ou mappe où l’application peut remplacer l’expérience par défaut par un adaptées à l’application.

> [!IMPORTANT]
> Bien qu’il soit possible d’ajouter des éléments interactifs tels que `UIButtons` ou `UITextFields` à l’Extension de l’interface utilisateur intention `UIViewController`, ils sont strictement interdite en tant que l’interface utilisateur intention dans non interactive et l’utilisateur ne sera pas en mesure d’interagir avec eux.

Il est totalement facultative pour l’application de fournir une Extension de l’interface utilisateur d’intention, étant donné que Siri contient un ensemble par défaut de l’interface utilisateur pour chaque type d’intention. En outre, les interfaces de l’interface utilisateur Intents sont uniquement disponibles pour certaines intentions que Apple a jugé peut s’avérer utiles à l’utilisateur.

## <a name="adding-sirikit-vocabulary"></a>Ajout de vocabulaire de SiriKit

La dernière partie de l’implémentation de SiriKit se trouve dans l’application en fournissant le vocabulaire nécessaire. De nombreuses applications ont des méthodes uniques de décrire les informations à l’utilisateur et les méthodes uniques que l’utilisateur fournit des informations à l’application.

Pour cette raison, Siri requiert une assistance de l’application pour comprendre les termes et expressions uniques à l’application. Certaines de ces expressions fera partie de l’application afin que chaque utilisateur sera connaître et comprendre les. D’autres seront encore uniques à un utilisateur donné de l’application.

### <a name="app-specific-vocabulary"></a>Vocabulaire de spécifiques de l’application

Le vocabulaire spécifique d’application définit les mots et expressions spécifiques qui seront connues à tous les utilisateurs de l’application, telles que les types de véhicules ou noms d’entraînement. Ils font partie de l’application, elles sont définies dans un `AppIntentVocabulary.plist` fichier en tant que bundle de l’application principale. En outre, ces mots et des expressions doivent être localisées.

Il existe plusieurs parties à un vocabulaire `AppIntentVocabulary.plist` fichier :

- **Application d’exemple utilise** -ils fournissent un ensemble de cas d’utilisation courants pour les requêtes que l’utilisateur peut faire de l’application. Exemple : *« Démarrer un entraînement avec MonkeyFit ».*
- **Paramètres** -ils fournissent un ensemble de types de paramètres non standard spécifiques à l’application. Par exemple, les noms de séances d’entraînement pour l’application MonkeyFit. Il s’agit de :
    - **Une expression** -autorise l’application de définir des termes uniques pour l’application. Par exemple : le type d’entraînement « Bananarific » pour l’application MonkeyFit. 
    - **Prononciation** -donne des indications de prononciation à Siri comme une simple orthographe phonétique une expression donnée. Par exemple, « ba nana ri fic ».
    - **Exemple** -fournit un exemple d’utilisation de l’expression donnée dans l’application. Par exemple, *« Démarrer un Bananarific dans MonkeyFit »*.

Pour plus d’informations, consultez le site d’Apple [référence de Format de fichier de vocabulaire application](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CustomVocabularyKeys.html#//apple_ref/doc/uid/TP40016875-CH10-SW1).

### <a name="user-specific-vocabulary"></a>Vocabulaire spécifique d’utilisateur

Le vocabulaire spécifique utilisateur va fournir des mots ou expressions qui sont uniques à des utilisateurs individuels de l’application. Il seront fournis lors de l’exécution à partir de l’application principale (pas les Extensions d’application) comme un ensemble ordonné de termes classés dans une priorité d’utilisation plus significative pour les utilisateurs, les termes les plus importantes au début de la liste.

Examinons l’exemple de l’application MonkeyChat présentée ci-dessus. MonkeyChat conserve une liste de tous les contacts de l’utilisateur, qui enverront à Siri via le vocabulaire spécifique d’utilisateur. Il conserve également une liste des contacts plus récente 10 que messages, l’utilisateur a et possède un ensemble de contacts favori pour chaque utilisateur. Pour cet exemple, les contacts favoris doivent être au début de notre vocabulaire spécifique de l’utilisateur, suivie de contacts récents puis le reste des contacts de l’utilisateur.

Les types d’informations suivants sont pris en charge par le vocabulaire spécifique d’utilisateur :

- Noms de contact.
- Noms d’entraînement.
- Noms d’Album photo.
- Mots clés de photo.

Si l’application s’appuie sur le carnet d’adresses d’iOS, l’application n’aura pas à intervenir, que ces informations soient déjà disponibles pour Siri. L’application doit uniquement fournir des noms de contact si l’application a sa propre base de données unique de contacts.

Lorsque vous concevez le vocabulaire, uniquement fournir les valeurs requises que les utilisateurs connaissent et s’intéressent à. Évitez de fournir des informations telles que les numéros de téléphone ou adresses de messagerie.

L’application doit également mettre à jour rapidement Siri lorsque vocabulaire spécifique utilisateur change. Les utilisateurs sont habitués à demander des informations à partir de Siri l’instant, il a été ajouté à leur appareil iOS. Par exemple, si l’utilisateur ajoute un nouveau contact dans l’application, envoyer ces informations à Siri dès que l’utilisateur enregistre.

Plus important encore, l’application _doit_ supprimer rapidement des informations à partir du vocabulaire Siri dans la mesure où un utilisateur peut devenir non plus contrarier si leur suppression d’un élément d’information mais Siri a été le reconnaître toujours heures ou jours plus tard.

> [!IMPORTANT]
> L’application doit supprimer tous le vocabulaire spécifique d’utilisateur de Siri si l’utilisateur choisit de réinitialiser l’application ou si leur déconnexion.

## <a name="sirikit-permissions"></a>Autorisations de SiriKit

La dernière pièce du SiriKit est centrée sur les autorisations. Tout comme l’utilisation d’autres fonctionnalités d’e/s (par exemple, des Photos, appareil photo ou des Contacts), les utilisateurs doivent accorder l’autorisation explicite pour l’application communiquer avec Siri.

L’application est en mesure de fournir une chaîne définissant quelles informations il fournir à Siri et donner la raison quant à la raison pour laquelle l’utilisateur doit accorder cet accès. 

Apple suggère que l’application doit demander l’autorisation de l’utilisateur à utiliser Siri la première fois que l’utilisateur ouvre l’application une fois qu’ils ont mis à niveau vers iOS 10. Il s’agit pour que les utilisateurs savoir sur l’intégration de Siri et qu’il peuvent pré-approuvée utilisation avant de procéder à leur première demande.

## <a name="sirikit-and-maps"></a>SiriKit et mappages

SiriKit fait partie intégrante d’e/s et utilise une infrastructure plus grande intentions ajoutée à iOS 10. Le framework intentions a été conçu pour partager les intentions et les actions communes et partagées avec d’autres parties du système.

Le framework intentions va au-delà de simplement Siri intégration et fournit d’autres fonctionnalités telles que l’intégration de Contacts où l’application peut devenir la téléphonie de la valeur par défaut ou une application de messagerie des contacts spécifiques. Intentions fournissent également une intégration approfondie avec CallKit pour fournir aux utilisateurs la meilleure expérience VOIP possible.

L’application de mappages dans iOS 10 a ajouté des fonctionnalités telles que le partage de tour dans lequel l’utilisateur peut réserver un tour directement à l’intérieur de l’interface utilisateur de cartes. SiriKit fournit un point d’extension courants avec des cartes donc accrochez-vous les intentions partage (et autres) peuvent être partagées entre Siri et cartes. 

Cela signifie que si l’application a adopté les extensions de SiriKit, il permet également d’établir l’intégration des cartes. 

## <a name="designing-a-great-siri-experience"></a>Conception d’une excellente expérience de Siri

Conception d’une expérience utilisateur satisfaisante lors de l’intégration d’une application dans Siri est différent de la conception d’une interface utilisateur de grande application. Contrairement aux situations normales où l’utilisateur interagit avec l’application directement suite écran, lors de l’utilisation de Siri il sont souvent lorsque aucune interface visuelle n’est visible à tous. Par exemple, lorsque l’utilisateur a démarré la conversation avec *« Hey Siri »*.

### <a name="how-siri-helps-the-developer"></a>Comment Siri permet au développeur

Lorsque vous concevez des interactions d’une application avec Siri, vous allez générer l’application un *Interface conversationnel*, ce qui signifie que le contexte est dérivé de la conversation Siri rencontre avec l’utilisateur sur le nom de l’application.

En l’absence d’une représentation visuelle, l’utilisateur doit conserver une trace de l’affichage des informations dans leur tête. Pour cette raison, Siri présente un système informations minimales requises pour atteindre la tâche de l’utilisateur souhaitant d’accomplir.

L’Interface de conversation est mise en forme par les questions et réponses de l’utilisateur et le Siri pendant la conversation. Par conséquent, il est important de réfléchir à la façon dont Siri pose des questions et répond lors de la conception de cette interface.

Prenons l’exemple suivant de l’utilisateur qui crée un message, Siri peut répondre à la question, *« Vous êtes prêt à les envoyer ? »* . L’utilisateur pourrait répondre de différentes manières, telles que *« Envoyer »*, *« Annuler »* ou même une chose totalement sans rapport avec cette question. Quelle que soit la façon dont la conversation se déroule, Siri gérer pour l’application et uniquement envoyer les informations pertinentes dès que possible.

Il existe plusieurs façons différentes, que l’utilisateur peut initier une conversation avec Siri :

- En reprenant l’appareil, en appuyant sur le bouton Accueil. Dans cette situation Siri présente plusieurs interfaces visual et moins réponses verbales.
- En disant *« Hey Siri »* et démarrage d’une conversation Installation mains libres. Dans cette situation Siri sera moins visual et plus verbale.
- À l’aide des fonctionnalités d’accessibilité tels que bluetooth est activé auditifs où l’interface utilisateur est adaptée pour un utilisateur ayant des besoins particuliers.
- À l’aide de la lecture de voiture où l’utilisateur doit conserver leur attention concentrions sur l’en conservant les distractions au minimum.

### <a name="how-the-developer-helps-siri"></a>Comment le développeur vous aide à Siri

Quand vous intégrez une application avec Siri, le développeur doit tester souvent cette intégration et vous assurer qu’ils sont établit plusieurs demandes différentes en demandant la même partie de la tâche ou des informations de différentes façons autant que possible.

Dans la mesure où aucun deux ne personnes réflexion similaire, il est essentiel que le développeur obtient autant différents bêta-testeurs que possible pour aider à affiner l’intégration de Siri. Les utilisateurs peuvent demander des informations ou effectuer des demandes de façons le développeur jamais cependant d’et ce réglage peut permettre de garantir que le groupe plus large d’utilisateurs ont une grande expérience à l’aide de leur application avec Siri.

Dans des situations différentes et environnements de test. Lancer les conversations avec Siri dans toutes les méthodes possibles garantir que ces conversations demeurent fluide et naturelles. Tester dans des emplacements où l’utilisateur serait probablement être à l’aide de l’application, comme dans une salle de sport encombré.

Assurez-vous que l’application fournit toutes les informations que Siri doit représenter correctement la demande et le résultat à l’utilisateur. Cela est particulièrement vrai lors de l’utilisation de Siri dans une situation Installation mains libres.

### <a name="siri-design-guidelines"></a>Instructions de conception de Siri

N’oubliez pas que Siri rencontre une conversation avec l’utilisateur de la part de l’application. Le développeur souhaite êtes pas certain que cette conversation reste aussi fluide et naturel que possible.

Comme ils le feraient avec toute conversation importante, le développeur doit vérifier les éléments suivants :

- Que l’application est prête pour la conversation.
- Que l’application écoute exactement ce que l’utilisateur tente de faire.
- Que l’application pose les questions appropriées aux moments appropriés.
- Que l’application répond à la demande avec les informations de la recherche de l’utilisateur.

#### <a name="preparing-for-the-conversation"></a>Préparation de la Conversation

La première chose à retenir est que les utilisateurs de l’application ne vont pas être exactement comme le développeur. Ils peuvent être à partir de différents arrière-plans, parlent différentes langues ou avez des besoins particuliers lorsque vous travaillez avec l’application.

En outre, étant donné que le développeur conçu et créé l’application, ils ont profond, une connaissance approfondie de l’application et son fonctionnement interne et fonctions d’un utilisateur typique n’a pas. Par conséquent, le développeur peut demander la demande de Siri différemment d’un utilisateur normal.

C’est pourquoi il est essentiel d’avoir plusieurs personnes que possible interagir avec l’application par le biais de Siri. Les utilisateurs peuvent effectuer des requêtes de l’application par le biais de Siri le développeur n’a jamais pensé que d’ou de façons que le développeur n’a pas prendre en compte.

#### <a name="ensure-the-app-is-a-good-listener"></a>Vérifiez que l’application est une bonne écouteur

Le développeur doit s’assurer que l’application est un bon port d’écoute et reçoit les spécificités de la conversation qui répondent aux attentes de l’utilisateur. Mais il est également possible qu’ils n’avez pas fourni toutes les informations que l’application a besoin pour effectuer la tâche demandée.

Il existe plusieurs façons que l’application peut gérer cette situation :

- **Choisir une bonne valeur par défaut pour la valeur manquante** , par exemple un tour d’application de partage peuvent être par défaut à l’emplacement actuel de l’utilisateur si elles n’ont pas spécifient où il souhaite être récupéré à partir de.
- **Obtenues** -à l’aide des informations spécifiques que l’application a collectées sur l’utilisateur, l’application peut être en mesure de marque et obtenues sur les informations manquantes, telles que le remplissage d’un numéro de téléphone portable manquant à partir des informations de contact de l’utilisateur. Toutefois, veillez à éviter les surprises incorrectes, telles que l’option la plus coûteuse, etc. de prélèvement.
- **Invite pour plus d’informations** -l’application peut avoir Siri inviter l’utilisateur à la valeur manquante. Toutefois, la clé ici consiste à conserver les conversations simple et au point. Les utilisateurs seront rapidement sont frustrés si elles doivent répondre à plusieurs questions pour atteindre leur demande.
- **Gérer correctement désinformation** -l’utilisateur peut fournir une valeur qui n’était pas attendu. l’application ou qu’il ne peut pas gérer dans le contexte donné. Assurez-vous que l’application est associée à cette situation à l’utilisateur d’une manière qui rend clair et facile pour les corriger.

Lorsque l’application est présentée avec une valeur unique qui est en question, la meilleure façon de gérer cette situation consiste à avoir Siri à demander à l’utilisateur à confirmer l’opération. Par exemple, *« Voulez-vous dire Bobo le grand ? »* , laquelle ils peuvent répondre à une simple réponse Oui ou non.

Lorsqu’il existe une situation dans laquelle plusieurs choix possibles peuvent être adaptés à une valeur unique, levée d’ambiguïté est la méthode de gestion par défaut. Dans cette situation Siri peut inviter l’utilisateur avec jusqu'à dix possibles d’options. Exemple :

```csharp
Who do you want to send the message to?

* Bobo the Great!
* Bobo Jr.
* Little Bobo
```

Toujours en question, ont Siri inviter l’utilisateur à fournir une réponse totalement nouvelle, plus spécifique pour une valeur donnée.

#### <a name="request-final-confirmation"></a>Demander Confirmation finale

Avant que l’application effectue réellement la tâche pour répondre à la demande de l’utilisateur, Siri vérifiera avec l’Extension d’application pour vous assurer que tout est en place. Par exemple, l’utilisateur n’a pas assez d’argent dans leur compte pour le paiement demandé ?

En outre, l’application doit s’assurer qu’il fournit toutes les informations possibles à Siri afin de pouvoir présenter à l’utilisateur et vérifiez que la tâche sur le point d’être effectuée répond à leurs attentes.

Une fois que l’utilisateur a confirmé la demande et l’application a effectuée, l’application doit à nouveau de vous assurer qu’il a fourni tous les résultats vers Siri afin qu’il puisse les associer à l’utilisateur.

#### <a name="responding-to-the-request"></a>Répondre à la demande

Siri a plusieurs Interfaces utilisateur intégrés pour chacun des domaines et des Actions qu’il connaît. Toutefois, le cas échéant, l’application peut fournir une Extension de l’interface utilisateur personnalisée intention pour enrichir l’expérience utilisateur en présentant la personnalisation de l’application et de l’interface utilisateur ou de plus d’informations que n’était présent dans la demande.

Ceci dit, retenue doit être utilisé lors de la conception des interfaces personnalisées de Siri. En règle générale, l’utilisateur, si vous voulez obtenir une tâche spécifique plue rapidement que possible et ne souhaite pas être surchargé avec les informations inutiles.

Il convient également pour vous assurer que l’interface utilisateur personnalisée se présente et répond correctement dans tous les appareils iOS différents et les orientations que l’utilisateur peut avoir ou être à l’aide de l’appareil.

Le cas échéant, utilisez l’API de SiriKit pour masquer toutes les informations redondantes déjà présentes dans la valeur par défaut Siri UI. Encore vous assurer que l’application est toujours en fournissant les informations à Siri afin qu’il peut le présenter verbalement dans un cas mains gratuit.

Il peut exister des situations où Siri lancera l’application pour répondre à la demande de l’utilisateur, telles que présentant les photos de que l’utilisateur a demandé. Dans ces situations, ne pas surpris par l’utilisateur. Afficher les informations attendues sans avoir des étapes intermédiaires ou davantage d’interaction. Ne jamais afficher des informations ou effectuer une tâche de que l’utilisateur n’est pas attendu.

### <a name="polishing-the-design"></a>Finition de la conception

Il existe plusieurs étapes Apple suggère de peaufiner la conception des Interfaces conversationnelles. Est tout d’abord, en fournissant claire et concises, vocabulaire et l’utilisation exemples à Siri.

L’une des façons qu’un utilisateur détecte l’application est en lançant une conversation avec Siri et en demandant, *« Que pouvez-vous faire ? »* Siri affichera plusieurs choses, il peut le faire, y compris les application du développeur et le héros exemple cas d’usage auxquels il a fourni par le biais de son `plist` fichier.

Guide pratique pour écrire de bons exemples de cas d’utilisation

- Vérifiez que les exemples incluent le nom de l’application.
- Gardez à l’exemple à court et à point.
- Fournit plusieurs exemples pour chacun des intentions qui prend en charge de l’application.
- Hiérarchiser les intentions et les exemples dans les selon les cas d’utilisation courants pour l’application.
- Assurez-vous que l’application fournit des exemples localisées.
- Vérifiez que chaque exemple donné fonctionne comme prévu dans l’application.
- Éviter d’adressage dans les exemples, donc n’incluez pas le texte comme Siri *« Hey Siri... »*
- Éviter toute pleasantries inutiles tels que *«, »* ou *« Merci »*.

Prenez le temps approprié pour Explorer et tester sur comment l’application peut mettre en forme la conversation Siri rencontre avec l’utilisateur en son nom. Veillez à communiquer avec les utilisateurs classiques tout au long du processus, en tant que leurs interactions avec et les attentes de l’application peuvent changer au fil du temps.

Pensez toujours à tester l’application dans différentes situations et toutes les différentes méthodes pour appeler une conversation avec Siri. Test dans des emplacements du monde réel de l’utilisateur peut être à l’aide de l’application, en dehors de l’office et au support.

Vous efforcer d’utiliser les conversations avec Siri (pour le compte de l’application) fluide, naturel et « sentiront simplement ». 

## <a name="summary"></a>Récapitulatif

Cet article a présenté les concepts clés nécessaires pour utiliser SiriKit et indiqué qu’il peut interagir avec les applications Xamarin.iOS pour fournir des services qui sont accessibles à l’utilisateur à l’aide de Siri et l’application Maps sur un appareil iOS.




## <a name="related-links"></a>Liens associés

- [Exemple de ElizaChat](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [Guide de programmation de SiriKit](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [Référence de Framework intentions](https://developer.apple.com/reference/intents)
- [Référence de Framework d’interface utilisateur Intents](https://developer.apple.com/reference/intentsui)
