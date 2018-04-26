---
title: Application localiser l’Interface utilisateur
ms.prod: xamarin
ms.assetid: CC6847B2-23FB-4EDE-9F7E-EF29DD46A5C5
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 52f4c51258d1b635adcd697bccb7dd553acc0d5e
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/26/2018
---
# <a name="localization"></a>Localisation

Ce guide présente les concepts qui sous-tendent *internationalisation* et *localisation* et des liens vers des instructions sur la façon de générer des applications mobiles Xamarin à l’aide de ces concepts.

Si vous souhaitez ignorer directement dans les détails techniques de la localisation des applications Xamarin, démarrez avec un de ces articles de procédures spécifiques à la plateforme :

- [**Xamarin.Forms** ](~/xamarin-forms/app-fundamentals/localization.md) localisation interplateforme à l’aide de fichiers RESX.
- [**Xamarin.iOS** ](~/ios/app-fundamentals/localization/index.md) localisation de la plateforme native.
- [**Xamarin.Android** ](~/android/app-fundamentals/localization.md) localisation de la plateforme native.

## <a name="i18n-and-l10n"></a>I18N et L10n

*Internationalisation* consiste à rendre votre code capable d’afficher différentes langues et l’adaptation de son affichage pour différents paramètres régionaux (par exemple, le nombre et date de mise en forme). Cela est également appelé *globalisation*.

*Localisation* est l’étape qui suit : création des ressources (telles que des chaînes et des images) pour chaque langue et regroupez-les avec l’application internationalize.

Internationalisation qu'il est souvent abrégé i18n – raccourcie pour les 18 lettres entre « i » et « n ». Localisation est de même raccourcie L10n, pour les 10 lettres entre « L » et « n ».

## <a name="overview"></a>Vue d'ensemble

Ce document présente les concepts associés d’internationalisation et localisation, et comment elles s’appliquent au développement d’applications mobiles en général.
Lors de la conception et création d’une application, les éléments précédemment avez peut-être codé en dur, mais qui doivent paramétrables pour localisation incluent :

-   Dispositions d’écran et du texte,
-   Icônes, des graphiques et des couleurs,
-   Fichiers vidéo et audio,
-   Texte dynamique et la mise en forme texte (par exemple, des nombres, devises et dates),
 - Changements de disposition pour les langages de droite à gauche (RTL), et
-   Tri des données.

Quels que soient les plateformes mobiles, votre application cible les conseils suivants vous aideront à créer une application localisée de haute qualité.


## <a name="design-considerations"></a>Considérations de design

Architecture d’une application afin qu’il soit possible de localiser son contenu, internationalisation est appelée. Internationalisation correctement est plus que simplement permettant de langue différente des chaînes à charger lors de l’exécution, une application bien conçue doit autoriser pour toutes les ressources à modifier en fonction de la langue et paramètres régionaux (y compris les images, des sons et des vidéos) et peut s’adapter mise en forme et la disposition pour s’adapter à différentes chaînes de taille.

Cette section présente certaines considérations de conception à prendre en considération lors de la création d’une application internationalisée.

### <a name="layouts-and-string-length"></a>Dispositions et la longueur de chaîne

Chaînes chinois et le japonais peuvent être très bref – parfois un ou deux caractères peuvent être suffisamment explicites pour une étiquette de champ d’entrée.

(Chaînes allemands par exemple) peuvent être très longs ; Parfois, un mot relativement court en anglais devient très long dans d’autres langages – soit devenant tronqué ou autre redistribution de façon inattendue la mise en page.

Comparer les longueurs de chaîne pour les quelques éléments sur l’écran d’accueil en anglais, allemand et japonais iOS :

[![](localization-images/language-compare-sml.png "Longueur de la chaîne japonais allemand vs")](localization-images/language-compare.png#lightbox)

Notez que **paramètres** en anglais (8 caractères) requiert 13 caractères pour la traduction en allemand, mais seulement 2 caractères en japonais.

Dispositions où l’étiquette de l’affichage et d’un champ d’entrée sont côte à côte sont difficiles à utiliser lorsque la longueur d’étiquette peut varier considérablement. Une mise en page dans laquelle l’étiquette est affichée au-dessus d’un champ est souvent plus facile à localiser parce que la largeur totale de l’écran est disponible pour l’étiquette et l’entrée.

En règle générale, si vous générez des dispositions fixes (en particulier les éléments côte à côte) autoriser au moins 50 % de largeur plus que vos chaînes en anglais nécessitent des étiquettes de texte. Cela ne sera pas résoudre tous les problèmes, mais fournit une mémoire tampon qui fonctionne dans de nombreux cas.

### <a name="input-validation"></a>Validation d’entrée

Méfiez-vous des hypothèses lors de l’écriture des règles de validation. Il peut sembler valide pour un champ de texte d’entrée à « exiger » au moins trois caractères en anglais, puisque très rarement une seule lettre n’a aucune signification. Dans le chinois et le japonais cependant un seul caractère peut être une entrée valide, et une validation de message « au moins 3 caractères est requis » n’est pas pertinent pour ces langues.

Autres tâches apparemment simples telles que la validation d’une adresse de messagerie ou URL du site Web deviennent plus complexe avec les caractères ne sont pas limités au sous-ensemble d’ASCII.

Écrire vos règles de validation avec internationalisation esprit – choisissez les règles les moins restrictives, ou écrire la logique pour qu’il fonctionne différemment pour chaque langue.

### <a name="images-and-color"></a>Couleurs et images

Pas à chaque image doit modifier en fonction des choix de la langue d’un utilisateur. Plusieurs icônes ou photos seront adapté à tous les utilisateurs, ils parlent langage a pas d’importance.
Sens de certaines ressources à localiser, telles que :

 - Images représentant des personnes ou des emplacements spécifiques à votre application peut-être avoir l’impression plus pertinente par rapport aux utilisateurs si elle présente des personnes/emplacements.
 - Icônes – certains iconographie peut être spécifiques à la culture et rendre votre application plus facile à utiliser en localisant l’imagerie afin de refléter la présentation locale.
 - Couleurs – certaines cultures comprennent couleurs différemment : rouge, cela peut signifier avertissement dans une région, mais bonne chance dans un autre. Vérifiez auprès de leur langue maternelle lors de la conception de votre application pour déterminer si vous êtes nécessaire de créer un mécanisme permettant de localiser des couleurs.


### <a name="videos-and-sound"></a>Vidéos et audio

Vidéos et des clips vidéo ou audio défis particuliers présentes lors de la localisation d’une application, car il est relativement facile d’obtenir des chaînes traduites, enregistrement voiceover plusieurs effectue le suivi peuvent être coûteuse et difficile.

Plusieurs copies des fichiers vidéos et audio peuvent augmenter considérablement de la taille de votre application (en particulier si vous effectuez la localisation dans un grand nombre de langues ou que vous avez un grand nombre de fichiers multimédias). Vous pouvez envisager de téléchargement que les composants de la langue requise une fois que l’utilisateur a installé votre application, mais cela peut également entraîner une expérience utilisateur médiocre sur les réseaux lents.

Il existe souvent plusieurs façons de résoudre les problèmes de localisation : la chose la plus importante consiste à prendre en compte initial et assurez-vous que votre application est conçue pour prendre en charge d’eux.


### <a name="dates-times-numbers-and-currency"></a>Dates, heures, nombres et les devises

Si vous utilisez des fonctions de mise en forme .NET, n’oubliez pas de spécifier la culture afin que les séparateurs décimaux sont analysées correctement (et éviter les exceptions de conversion sont levées). Par exemple 1,99 et 1,99 sont des représentations décimales valides en fonction de vos paramètres régionaux.

Lorsque les données provient d’une source connue (ie. à partir de votre propre code ou un service web que vous contrôlez) vous pouvez coder un identificateur de culture qui correspond à la mise en forme telles que InvariantCulture qui fonctionne pour la mise en forme de langue anglais standard.

```csharp
double.Parse("1,999.99", CultureInfo.InvariantCulture);
```

Si les données sont en cours d’entrée par l’utilisateur de l’application, l’analyser à l’aide d’une instance CultureInfo qui reflète les paramètres régionaux :

```csharp
double.Parse("1 999,99", CultureInfo.CreateSpecificCulture("fr-FR"));
```

Consultez le [l’analyse de chaînes numériques](http://msdn.microsoft.com/library/xbtzcc4w(v=vs.110).aspx) et [l’analyse de chaînes de Date et heure](http://msdn.microsoft.com/library/2h3syy57(v=vs.110).aspx) articles MSDN pour plus d’informations.

<a name="rtl" />

### <a name="right-to-left-rtl-languages"></a>Langues de droite à gauche (RTL)

Certains langages, tels que l’arabe et hébreu Ourdou (par exemple), sont lues de droite à gauche.
Les applications qui prennent en charge ces langues doivent utiliser des modèles d’écran qui s’adaptent pour les lecteurs de droite à gauche, par exemple :

 - Texte doit être aligné à droite.
 - Étiquettes doivent apparaître à droite des champs d’entrée.
 - Emplacement du bouton par défaut est généralement inversée.
 - Le glissement de navigation hiérarchique et animation (et autres métaphores de navigation et les animations) qui utilisent la direction de contexte doit également être annulée.

IOS et Android prennent en charge les dispositions de droite à gauche et le rendu des polices, des fonctionnalités intégrées qui permettent d’apporter les modifications ci-dessus. Xamarin.Forms ne prend pas en charge actuellement automatiquement le rendu de droite à gauche.

### <a name="sorting"></a>Tri

Différentes langues définissent l’ordre de tri de leur alphabet différemment, même lorsqu’ils utilisent le même jeu de caractères.

Consultez le [détail de comparaison de chaînes](http://msdn.microsoft.com/library/dd465121(v=vs.110).aspx#the_details_of_string_comparison) dans [meilleures pratiques pour l’utilisation de chaînes dans le .NET Framework](http://msdn.microsoft.com/library/dd465121(v=vs.110).aspx) pour obtenir un exemple où langue (CultureInfo) affecte l’ordre de tri.

Il est improbable que les fonctionnalités de base de données intégrée sur les plateformes mobiles prendront en charge tri spécifiques au langage classement afin de vous devrez peut-être implémenter le code supplémentaire dans votre logique métier.

### <a name="text-search"></a>Recherche de texte

Veillez à écrire et tester votre algorithme de recherche avec plusieurs langues à l’esprit. Éléments à prendre en considération sont les suivantes :

 - Saisie semi-automatique – si vous avez créé une fonction de saisie semi-automatique, assurez-vous que sources de suggestions relatives à la langue de l’utilisateur.
 - Requête correspondant aux données – recherche requêtes entrés dans spécifique à un langage être exécutées sur tout le contenu écrit dans ce langage, ou sur tout le contenu de votre application ?
 - Recherche de radical – si votre recherche est conçue pour rechercher des mots similaires, les racines de word et les autres optimisations de recherche, est ces optimisations générées pour toutes les langues que vous prenez en charge ?
 - Tri – Assurez-vous que les résultats sont triés correctement (voir ci-dessus).


### <a name="data-from-external-sources"></a>Données provenant de sources externes

De nombreuses applications téléchargement des données provenant de sources externes, de Twitter et les flux RSS à la météo, des nouvelles ou des actions. Lors de l’affichage de ce à un utilisateur, vous devez envisager la possibilité que vous afficherez un écran d’informations sans intérêt ou illisibles leur.

Il existe quelques stratégies que vous permet de s’assurer de que votre application affiche des données relatives à l’utilisateur :

 - Sources différentes : votre application peut télécharger les données d’une source différente en fonction des paramètres régionaux ou linguistiques de l’utilisateur. Paramètres régionaux actualités, la météo et stock quotidiens pourraient convenir plus qu’un élément à partir d’un flux en Amérique du Nord.
 - Complet localisé – si vous affichez une photo de flux, ou un Twitter vous devez afficher les métadonnées (par exemple, le temps nécessaire) en langage son propre, même si le contenu proprement dit reste dans la langue d’origine.
 - Traduction : vous pouvez générer une option de traduction à votre application pour effectuer une traduction de données entrantes. Cela peut être automatique ou à la discrétion de l’utilisateur – simplement veillez à informer l’utilisateur si cela se déroule, étant donné que les traductions ne sont jamais parfaites !

Cela peut affecter également des liens externes aux pistes audio ou vidéos – lorsque vous concevez votre application veillez à anticiper approvisionnement traduite de contenu ou de s’assurer que les utilisateurs sont informés de manière par l’interface utilisateur lorsque le contenu n’est pas présenté dans leur langage.


### <a name="dont-over-translate"></a>Ne pas traduire excessive

Certaines chaînes de votre application ne peuvent pas besoin de traduction, ou au moins une attention particulière par le traducteur. Sont des exemples :

 - URL – si vous répertoriez une URL, il peut ou n'ayez pas besoin d’être ajustée par langue. Par exemple, facebook.com ne requiert pas qu’il détecte automatiquement la langue sur le site principal de traduction. Autres sites ont contenu spécifique aux paramètres régionaux et vous pouvez souhaiter proposer une URL différente, par exemple yahoo.com quelques yahoo.fr ou yahoo.it.
 - Numéros de téléphone : en particulier ceux avec des codes de pays différents ou des nombres pour les appelants qui parlent une langue particulière.
 - Détails du contact – adresses et autres informations peuvent varier par langue ou paramètres régionaux.
 - Marques et noms de produits – certaines chaînes n’avez pas besoin traduction, car elles sont toujours écrites dans la même langue.

Enfin, veillez à inclure des instructions détaillées pour le traducteur si certaines chaînes nécessitent un traitement spécial.


### <a name="formatted-text"></a>Texte mis en forme

N’est généralement pas un problème avec les applications mobiles, car les chaînes généralement ne sont pas mise en forme enrichie. Cependant, si le texte enrichi (par exemple, la mise en forme gras ou italique) est requise dans votre application vous assurer le traducteur sait comment pour la mise en forme des entrées, vos fichiers de chaînes stockent correctement, il est mis en forme correctement avant d’être affichées à l’utilisateur (c.-à-d. ne laissez pas accidentellement les codes de mise en forme eux-mêmes être présenté à l’utilisateur).



## <a name="translation-tips"></a>Conseils de traduction

Traduire les chaînes utilisées par une application est considérée comme faisant partie du processus de localisation. En général, cette tâche être externalisée à un service de traduction et effectuée par le personnel multilingue qui ne peut pas savoir votre application ou votre entreprise.

Les conseils suivants vous aideront à produire des chaînes qui sont plus faciles à traduire correctement et par conséquent, améliorer la qualité de vos applications localisées.



### <a name="localize-complete-strings-not-words"></a>Localiser des chaînes complètes, pas les mots

Parfois, les développeurs adopter l’approche de la tentative de spécifier des mots ou la phrase « des extraits de code » afin que qu’ils peuvent réutiliser tout au long de l’application. Par exemple, pour le texte « vous avez 5 messages. » ils peuvent spécifier les chaînes suivantes pour la traduction

**Mauvais**:

```csharp
"You have"
"no"
"message"
"messages"
```

et tentez ensuite de créer la corriger l’expression à la volée dans le code à l’aide de la concaténation de chaînes :

**Mauvais**:

```csharp
"You have" + " " + numMsgs + " " + "messages"
"You have" + " no " + "messages"
```

**Cette méthode est déconseillée** , car il ne fonctionnera pas nécessairement pour tous les langages et sera difficile pour le traducteur comprendre le contexte de chaque segment court. Cela entraîne également réutilisation des chaînes traduites, ce qui peuvent entraîner des problèmes plus tard si elles sont utilisées dans des contextes différents (et puis mis à jour).


### <a name="allow-for-parameter-re-ordering"></a>Autoriser pour le paramètre réorganisation

Certains langages de programmation nécessitent une syntaxe supplémentaire pour spécifier l’ordre des paramètres dans une chaîne, mais .NET prend déjà en charge le concept des espaces réservés de numérotées, par conséquent

**Bonne**:

```csharp
"a {0} b {1} cde {3}"
```

peut être traduit les éléments suivants (où est modifié la position et l’ordre des espaces réservés)

```csharp
"{2} {3} f g h {0}"
```

et les jetons sont classés en tant que le traducteur prévu. Veillez à inclure une explication de ce que contient chaque espace réservé lors de l’envoi de la chaîne à un convertisseur.


### <a name="use-multiple-strings-for-cardinality"></a>Utiliser plusieurs chaînes pour la cardinalité

Évitez les chaînes comme `"You have {0} message/s."` permet de fournir une meilleure expérience utilisateur des chaînes spécifiques pour chaque état :

**Bonne**:

```csharp
"You have no messages."
"You have 1 messages."
"You have 2 messages."
"You have {0} messages."
```

Vous devez écrire du code dans votre application pour évaluer le nombre affiché et choisissez la chaîne appropriée. Certaines plateformes (y compris les iOS et Android) ont des fonctionnalités intégrées pour choisir automatiquement la chaîne au pluriel meilleures selon les préférences pour la langue/région actuelle.


### <a name="allowing-for-gender"></a>Autoriser pour le sexe

Parfois, les langues latines utilisent des mots différents selon le sexe de l’objet. Si votre application connaît sexe, vous devez autoriser les chaînes traduites en conséquence.

Il est également le cas le plus évident même en anglais, où les chaînes font référence à une personne spécifique ou d’un utilisateur de votre application. Par exemple, certains sites affichent les messages comme `"Bob commented on his post"` vous avez besoin de chaînes pour un sexe masculin, féminin et non binaires ou inconnu :

**Bonne**:

```csharp
"{0} commented on his post"
"{0} commented on her post"
"{0} commented on their post"
```

### <a name="dont-reuse-strings"></a>Ne réutilisez pas des chaînes

Ou, plus précisément, ne réutilisez pas des chaînes uniquement, car elles sont similaires, lorsque la chaîne a un objectif différent ou une signification particulière.

Par exemple : imaginez que vous avez un commutateur d’activation/désactivation dans votre application et le contrôle de commutateur doit être le texte pour 'on' et 'off' doit être localisée. Vous également Affrichez la valeur de ce paramètre ailleurs dans l’application dans une étiquette de texte. Vous devez utiliser des chaînes différentes pour l’affichage du commutateur par rapport à l’état du commutateur (même si elles sont la même chaîne dans votre langue par défaut) – par exemple :

-   « Dans la section » – affiché sur le commutateur
-   « Désactivé » – affiché sur le commutateur
-   « Dans la section » – affichés dans une étiquette
-   « Désactivé » – affichés dans une étiquette

Cela offre une souplesse maximale pour le traducteur :

-   Pour des raisons de conception, peut-être que le commutateur lui-même utilise les minuscules « on » et « off », mais l’étiquette d’affichage utilise la majuscule « On » et « Off ».
-   Certains langages peut-être la valeur du commutateur abrégé pour tenir dans le contrôle d’interface utilisateur, alors que la compléter le mot (traduit) peut apparaître dans l’étiquette.
-   Vous pouvez également, pour certaines langues le rendu de votre commutateur peut être utiliser « I » et « O » pour une connaissance de la culture, mais vous pouvez toujours l’étiquette à lire « On » ou « Off ».

### <a name="translation-services"></a>Services de traduction

#### <a name="machine-translation"></a>Traduction automatique

Pour générer des fonctions de traduction dans votre application, envisagez la [Azure traducteur texte API](https://azure.microsoft.com/en-au/services/cognitive-services/translator-text-api/).

À des fins de test vous pouvez utiliser un des nombreux outils de traduction en ligne pour inclure du texte localisé dans votre application au cours du développement :

- [Bing Translator](https://www.bing.com/translator/)
- [Traduire de Google](http://translate.google.com/)

Il existe de nombreux autres disponibles. La qualité de la traduction automatique n’est pas considérée comme suffisant pour libérer une application sans tout d’abord examiné et testé par les traducteurs professionnels ou leur langue maternelle.

#### <a name="professional-translation"></a>Traduction professionnelle

Il existe également des services de traduction professionnels qui prend les chaînes et les distribuer à leurs propres convertisseurs, vous offrant les traductions terminées pour un abonnement.

L’un des services plus connus est [LionBridge](http://www.lionbridge.com/). Services professionnels plus prendre en charge tous les types de fichier courantes, y compris les chaînes, XML, RESX et POT/bon de commande.


## <a name="summary"></a>Récapitulatif

Cet article a introduit certains des concepts que vous devez connaître avant d’internationalisation de votre application et puis à localiser vos ressources et également expliqué comment modifier les préférences de langue pour chaque plateforme.

Ces concepts peuvent être appliquées aux différentes techniques d’internationalisation spécifique à la plateforme et inter-plateformes qui ne sont possibles avec Xamarin.

Continuer à lire les détails techniques pour la plateforme qui que vous intéressez :

- [Xamarin.Forms](~/xamarin-forms/app-fundamentals/localization.md) localisation interplateforme à l’aide de fichiers RESX.
- [Xamarin.iOS](~/ios/app-fundamentals/localization/index.md) localisation de la plateforme native.
- [Xamarin.Android](~/android/app-fundamentals/localization.md) localisation de la plateforme native.



## <a name="related-links"></a>Liens associés

- [Vue d’ensemble de la localisation d’Apple](https://developer.apple.com/internationalization/)
- [Liste de vérification de localisation d’Android](http://developer.android.com/distribute/tools/localization-checklist.html)
- [Meilleures pratiques pour développer des Applications mondialisables (MSDN)](http://msdn.microsoft.com/library/w7x1y988%28v=vs.90%29.aspx)
