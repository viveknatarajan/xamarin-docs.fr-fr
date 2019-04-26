---
title: 'Partie 6 : Test et approbations de l’App Store'
description: Ce document décrit comment tester une application multiplateforme sur l’appareil, gérer les cas de test, automatiser les tests, exécuter des tests unitaires et fonctionne via le processus de soumission d’application.
ms.prod: xamarin
ms.assetid: 46E0578A-7EB9-C105-ABB0-A043E501F36B
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 0faf7c9e4ff7c96cdfd25ab6d6658726ef247b32
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61275405"
---
# <a name="part-6---testing-and-app-store-approvals"></a>Partie 6 : Test et approbations de l’App Store

## <a name="testing"></a>Test

De nombreuses applications (même les applications Android, sur certains magasins) devez réussir un processus d’approbation avant leur publication ; test est essentiel pour garantir votre application atteint le marché (sans parler de réussit avec vos clients). Test peut prendre différentes formes, à partir de l’unité du niveau développeur de test à la gestion des tests bêta dans une vaste gamme de matériel.

### <a name="test-on-all-platforms"></a>Tester sur toutes les plateformes

Il existe de légères différences entre ce que .NET prend en charge sur Windows phone, Tablet PC et appareils de bureau, ainsi que limitations sur iOS qui empêchent de code dynamique doit être généré à la volée. Soit le plan de test du code sur plusieurs plateformes pendant que vous développez, ou planifiez des temps de refactoriser et mettre à jour la partie du modèle de votre application à la fin du projet.

Il est toujours recommandé d’utiliser l’émulateur/de simulateur pour tester plusieurs versions du système d’exploitation et également différentes fonctionnalités/configurations d’appareils.

Vous devez également tester sur les appareils d’un matériel physique différent autant que possible.

#### <a name="devices-in-cloud"></a>Appareils dans le cloud

L’écosystème de téléphone et tablette mobile augmente tout le temps, ce qui empêche le tester sur le nombre sans cesse croissante d’appareils disponibles. Pour résoudre ce problème, un certain nombre de services offre la possibilité de contrôler à distance différents périphériques afin que les applications peuvent être installées et testées sans avoir à investir directement dans un grand nombre de matériel.

[Application Center Test](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest) offre un moyen simple de tester des applications iOS et Android sur des centaines d’appareils différents.

### <a name="test-management"></a>Gestion des tests

Lorsque vous testez des applications au sein de votre organisation ou de la gestion d’un programme bêta avec des utilisateurs externes, il existe deux défis :

- **Distribution** : la gestion de processus d’approvisionnement (en particulier pour les appareils iOS) et des versions mises à jour de logiciels aux testeurs.
- **Commentaires** – collecte des informations sur l’utilisation des applications et des informations détaillées sur les erreurs qui peuvent se produire.


Il existe un nombre de l’aide de services pour répondre à ces problèmes, grâce à une infrastructure qui est intégré à votre application pour collecter et consigner les erreurs et d’utilisation et également rationaliser le processus d’approvisionnement pour aider à inscrire et gérer des testeurs et leurs appareils .

[Visual Studio App Center](/appcenter/) offre une solution à ces problèmes, en fournissant la distribution de version test, le signalement des incidents et les informations d’utilisation des applications sophistiquées.

### <a name="test-automation"></a>Automatisation des tests

Xamarin [UITest](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest) peut être utilisé pour créer des scripts de test qui peuvent être exécutés localement ou chargés dans l’interface utilisateur automatisés [Test App Center](https://docs.microsoft.com/appcenter/test-cloud/).

## <a name="unit-testing"></a>Test unitaire

### <a name="touchunit"></a>Touch.Unit

Xamarin.iOS inclut une infrastructure de test unitaire appelée Touch.Unit qui suit le style de JUnit/NUnit écriture de tests.

Reportez-vous à notre [Unit Testing avec Xamarin.iOS](~/ios/deploy-test/touch.unit.md) documentation pour plus d’informations sur l’écriture des tests et en cours d’exécution Touch.Unit.

### <a name="andrunit"></a>Andr.Unit

Il existe un équivalent d’open source de Touch.Unit pour Android appelé Andr.Unit. Vous pouvez le télécharger à partir de [github](https://github.com/spouliot/Andr.Unit) et en savoir plus sur l’outil sur [ @spouliotdu blog](http://spouliot.wordpress.com/2011/10/30/andr-unit-joins-the-family/).

## <a name="app-store-approvals"></a>Approbations de l’App Store

Apple et Microsoft fonctionnent l’unique magasin sur leurs plateformes : l’App Store et place de marché respectivement. Les deux verrouiller leurs appareils et implémentent un processus de révision d’application rigoureuse pour contrôler la qualité des applications disponibles en téléchargement. Nature ouverte d’Android signifie qu’il existe un nombre d’options de magasin allant Play de Google, qui est largement disponibles et n’a aucun processus de révision, à l’App Store d’Amazon Android et de matériel spécifiques telles que des applications de Samsung qui ont une plus limitée de distribution et implémenter un processus d’approbation.

En attente pour une application à réviser peut être très stressants - pression signifie souvent la demande d’approbation avec très peu de marge d’erreur avant une date de lancement « cible ». Le processus lui-même peut prendre jusqu'à deux semaines et n’est pas nécessairement transparent : il est limité des commentaires sur la progression de votre application jusqu'à ce qu’il est enfin rejeté ou approuvé. Rejet peut signifier manquantes d’une fenêtre de marketing d’opportunité, surtout si cela se produit plusieurs fois, et passer des semaines entre votre date de lancement d’origine et lorsque l’application est enfin approuvée.

### <a name="be-prepared"></a>Préparez-vous

La première partie du Conseil : planifier le lancement de votre application à l’avance et de prendre en considération un rejet possible et de la nouvelle soumission. Chaque magasin, vous devez créer un compte avant de soumettre votre application : le faire dès que possible.
Pendant l’inscription de Google Play ne prend que quelques minutes si vos applications sont gratuites, le processus obtient beaucoup plus complexe si vous les vendre et que vous devez fournir des opérations bancaires et informations fiscales. Apple et les processus de Microsoft sont à la fois beaucoup plus complexe que de Google, il peut prendre une semaine ou plus pour que votre compte approuvé donc tient cette fois vos plans de lancement.

Une fois votre compte a été approuvé, vous êtes prêt à soumettre une application. Le processus réel pour soumettre des applications est traité dans la documentation suivante :

- [Publication sur iOS d’Apple App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Préparation d’une application pour Google Play](~/android/deploy-test/publishing/publishing-to-google-play/index.md)
- Les développeurs Windows doivent visiter le [centre de développement Windows](https://developer.microsoft.com/windows/windows-apps) pour en savoir plus sur l’envoi de leurs applications.

Le reste de cette section traite des choses que vous devez prendre en considération pour vous assurer de que votre application est approuvée sans les interruptions.

### <a name="quality"></a>Qualité

Il semble évident, mais les applications sont souvent obtenir rejetées, car elles ne répondent pas à un certain niveau de qualité : après tout, c’est la raison pourquoi les magasins organisés ont un processus d’approbation en premier lieu !

Incidents sont une raison courante de rejet. S’il est trop facile de concevoir des incidents de votre application, il a la garantie pour être rejetée. La plupart des développeurs ne soumettent leurs applications dans l’attente que vous leur auront se bloquer, mais ils font souvent. Testez votre application minutieusement avant de soumettre, se concentrant seulement se ne pas assurer que tout fonctionne, mais également que vous avez gérer des scénarios d’erreur mobiles courants tels que des problèmes de réseau et des contraintes de ressources telles que l’espace mémoire ou de stockage. Permet de tester le simulateur et les appareils physiques, quelle que soit la manière dont le code s’exécute dans un simulateur, seul un appareil permettant d’expliquer les performances réelles d’une application. Utilisez autant d’appareils différents que vous pouvez trouver et inscrire une équipe de bêta-testeurs si vous pouvez, à des services tiers peuvent aider à gérer les commentaires et la distribution bêta.

Tous les systèmes d’exploitation mobiles va arrêter une application qui ne commence pas assez rapidement. La longueur de la durée autorisée varie, mais en général les applications doivent viser à être réactif en quelques secondes et utiliser des tâches en arrière-plan pour effectuer tout le travail prendrait plus de temps. Les applications qui prennent trop de temps à se charger ou ne sont pas suffisamment réactive en cours d’utilisation régulière est rejetées. Toujours fournir des commentaires des utilisateurs lorsque quelque chose qui se passe en arrière-plan, ou l’application semble avoir été bloqué et une fois encore, essuyer un refus.

### <a name="check-your-edge-cases"></a>Vérifiez votre cas Edge

Interruption courants pour les développeurs ne parvient pas à la périphérie adresse, en particulier celles qui nécessitent la reconfiguration de leurs simulateur ou l’appareil pour tester correctement. Il peut être facile d’oublier que pas chaque client va « Allow » votre application pour accéder à leur emplacement, car une fois que le développeur a accepté la demande qu’une seule fois, il jamais est invité à nouveau. Autorisations et l’utilisation du réseau sont occupés spécifiquement sur pendant le processus d’approbation, ce qui signifie que le rejet peut entraîner une petite supervision dans ces domaines.

La liste suivante est un bon point de départ pour la vérification des cas marginaux peuvent avoir manqué :

- **Les clients peuvent « refuser » l’accès aux services** – en particulier dans iOS, l’accès aux données telles que les informations d’emplacement géographique sont fournies uniquement si l’utilisateur accorde l’autorisation de votre application. Les testeurs d’applications doivent fréquemment réinstallez l’application dans son état initial et interdire les demandes d’autorisation pour vous assurer de que l’application se comporte de manière appropriée. Activer/désactiver les autorisations sur et hors tension pour vérifier le comportement correct comme clients changeraient d’avis.
- **Les clients que vous soyez** – ne supposez pas qu’une application servira uniquement dans la ville ou le pays où elle a été développée ! Tout code qui fonctionne avec les coordonnées GPS, les valeurs de date et d’heure et les devises peut être affectée par emplacement et les paramètres régionaux du client. Toutes les plateformes offre un simulateur qui vous permettre de spécifier différents emplacements et paramètres régionaux - il permet de tester des emplacements dans l’hémisphère autres et avec les cultures mise en forme des dates et devises différemment. Valeurs de latitude et longitude peuvent être positif ou négatif, le séparateur décimal peut être un point ou une virgule et dates peuvent être mis en forme une multitude de façons différentes - préoccuper !
- **Lenteur des connexions réseau** – permet aux développeurs d’applications travaillent souvent dans un monde idéal de rapides, toujours travailler connectivité de réseau, ce qui n’est évidemment pas le cas dans le monde réel. Test avec une connectivité réseau lente (par exemple, une mauvaise connexion 3 G), ainsi qu’avec aucun accès réseau est essentielle pour garantir que vous ne sont pas livrés une application boguée. Le processus d’approbation est toujours inclure un test avec l’appareil dans le mode avion, assurez-vous que vous avez testé que vous-même.
- **Varie en fonction du matériel** : n’oubliez pas de tester sur le matériel plus ancien, plus lent que vous envisagez de prendre en charge. Il existe deux aspects qui peuvent affecter votre application : performances, ce qui peuvent être inutilisable sur un appareil plus anciens et prise en charge des fonctionnalités matérielles telles que d’une caméra, microphone, GPS, gyroscope ou un autre composant facultatif. Les applications doivent se dégrader normalement (et pas l’incident) quand un composant est indisponible.

### <a name="guidelines-are-more-than-just-a-guide"></a>Les instructions sont plus qu’un « guide »

Apple est connue pour être strict sur l’adhésion à leurs Human Interface Guidelines pour l’un des atouts de leur plate-forme est la cohérence (et l’augmentation perçue de facilité d’utilisation). Microsoft a adopté une approche similaire avec des applications Windows implémentant l’interface utilisateur de style Metro. Le processus d’approbation pour les deux plateformes implique votre application en cours d’évaluation pour le respect de la philosophie de conception pertinents.

Cela ne veut pas dire qu’innovation d’interface utilisateur n’est pas pris en charge ou encouragée, mais il existe certaines choses que vous « simplement ne doit pas faire » ou que votre application va être rejetée.

Sur iOS, cela inclut les retours d’icônes intégrés ou à l’aide d’autres métaphores bien établies de façon non cohérente ; par exemple, en utilisant l’icône « composer » pour autre chose que la création de contenu.

Les développeurs de Windows doivent être de même prudent ; une erreur courante ne peut pas prendre en charge correctement le matériel nouveau bouton conformément aux directives de Microsoft.

Encouragez vos concepteurs lire et suivre les instructions de conception pour chaque plateforme.

### <a name="implementing-platform-specific-features"></a>Implémentation des fonctionnalités spécifiques aux plateformes

Choses sont un peu plus strictes en matière d’à l’implémentation des services spécifiques à la plateforme, en particulier sur iOS. Pour éviter le rejet d’automatique par Apple, il existe des règles à suivre avec les fonctionnalités d’iOS suivantes :

- **Achats dans l’application** – les Applications ne doivent pas implémenter des mécanismes de paiement externe pour les produits numériques, y compris dans le jeu devise, les fonctionnalités de l’application, les abonnements magazines et beaucoup plus. applications iOS doivent utiliser le service d’Apple iTunes pour ce type de fonctionnalité. Il existe une faille de sécurité - applications telles que le lecteur Kindle et certaines applications basées sur abonnement vous permettent d’acheter du contenu ailleurs que vous associerez à un « compte », auquel vous pouvez ensuite accéder via l’application, toutefois dans ce cas l’application ne doit pas contenir des liens ou les références à la la sortie de l’application processus d’achat (ou une fois encore, elle allez rejetée).
- **la sauvegarde iCloud** – avec l’avènement des iCloud, les réviseurs d’Apple sont beaucoup plus strictes concernant la façon dont les applications utilisent les stockage (afin de garantir l’expérience du client de sauvegarde à distance est agréable). Applications que l’espace de stockage de sauvegarde-able déchets peut obtenir rejeté, par conséquent, utilisez le dossier du Cache en conséquence et suivez Apple d’autres instructions liées au stockage.
- **Newsstand** – applications magazines et journal sont parfaitement adaptés pour Newsstand d’Apple, toutefois, les applications doivent implémenter au moins une auto-renouvellement abonnement et la prise en charge du téléchargement en arrière-plan à approuver.
- **Mappe** – il est plus en plus courant d’ajouter des superpositions et autres fonctionnalités à maps mobiles, cependant veillez à ne pas d’altération de la carte « crédits » des informations (telles que le logo Google dans iOS5) car cela entraîne le rejet.

### <a name="manage-your-metadata"></a>Gérer vos métadonnées

Outre les problèmes techniques évidents qui peuvent entraîner une application rejetée, il existe des aspects plus subtiles de vos envois qui peut entraîner le rejet, notamment concernant les métadonnées (description, mots-clés et images marketing) que vous avez envoyer avec votre application pour l’affichage dans l’App Store ou de la place de marché.

- **IMAGERIE** : suivez les instructions de la plateforme pour les icônes d’application et stocker des photos. N’utilisez pas les images de marque déposées, nous avons vu les applications sont rejetées, car leurs icônes proposé un dessin d’un iPhone !
- **Des marques** : Évitez d’utiliser des marques déposées différent du vôtre. Applications ont été refusées pour mentionner des marques dans la description de l’application ou même dans les mots clés sur Apple App Store.
- **Description** – ne pas utiliser le mot « bêta » ou en aucune façon indiquent que l’application n’est pas prête pour les nombres premiers. Ne mentionnent pas autres plateformes mobiles (même si votre application fonctionne sur plusieurs plateformes). Plus important encore, assurez-vous que l’application effectue exactement ce que vous dites que c’est le cas. Si vous répertoriez un ensemble de fonctionnalités dans votre description, elle devraient être évidentes comment utiliser chacune de ces fonctionnalités, ou vous obtiendrez un rejet « fonctionnalités mentionnées dans la description de l’application ne sont pas implémentée ».

Placez autant d’efforts dans les métadonnées de l’application comme dans le développement et de test. Applications rejetées pour les infractions mineures dans les métadonnées il est utile de prendre le temps de le faire correctement.

### <a name="app-stores-not-for-everyone"></a>Magasins d’applications : Pas pour tout le monde

Le principal objectif des magasins sur chaque plateforme est la distribution de consommateur : la capacité d’atteindre des clients autant que possible. Cependant pas toutes les applications sont destinées à des consommateurs, il existe une croissance rapide base d’applications internes et plus extranet qui nécessitent une distribution limitée aux employés, fournisseurs ou clients. Ces applications ne sont pas « à la vente » et n’avez pas besoin d’approbation, depuis la distribution de contrôles de développeur à un groupe d’utilisateurs fermés.
Prise en charge pour ce type de déploiement varie selon la plateforme.

Android offre une flexibilité maximale à cet égard : applications pouvant être installées directement à partir d’une URL ou une pièce jointe (tant que la configuration de l’appareil le permet). Cela signifie qu’il est facile de créer et distribuer des applications d’entreprise internes ou publier des applications à des clients ou fournisseurs.

Apple fournit une option de déploiement interne pour les développeurs inscrits sur le site iOS Developer Enterprise Program, qui ignore le processus d’approbation App Store permet aux entreprises de distribuer des applications internes à leurs employés.
Malheureusement cette licence ne traite pas nécessaire de distribution d’applications de type extranet pour les autres groupes fermés de clients ou fournisseurs. [Enterprise (et Ad-Hoc) déploiement](~/ios/deploy-test/app-distribution/ipa-support.md)

### <a name="app-store-summary"></a>Résumé de l’App Store

Le processus de révision peut être fastidieuse, mais comme le reste du cycle de vie de développement vous pouvez aider à garantir la réussite avec une planification et une attention aux détails. Tout cela se résume à quelques étapes simples : lire et comprendre les instructions d’interface utilisateur vous devez respecter pour suivent les règles si vous implémentez les fonctionnalités spécifiques à la plateforme, effectuer des tests approfondis (puis effectuer d’autres tests) et enfin que vos métadonnées de l’application est correct avant d’envoyer.

Un dernier mot des conseils aux développeurs de publication sur Google Play : l’absence de processus d’approbation peut sembler longue facilite votre travail - mais vos clients seront encore plus élevés qu’une équipe de révision. Suivez ces instructions, car bien que votre application peut obtenir rejetée, sinon elle sera vos clients effectuant le rejet.
