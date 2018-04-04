#<a name="---"></a>---
titre : « Partie 6 – test et App Store approbations » ms.prod : xamarin ms.assetid : 46E0578A-7EB9-C105-ABB0-A043E501F36B ms.technology : xamarin-inter-plateformes auteur : asb3993 ms.author : amburns ms.date : 23/03/2017
---

# <a name="part-6---testing-and-app-store-approvals"></a>Partie 6 - test et les approbations de l’App Store


## <a name="testing"></a>Test

De nombreuses applications (même les applications Android, sur certains magasins) aura passer d’un processus d’approbation avant leur publication ; Par conséquent, de test est essentiel pour garantir votre application atteint le marché (sans parler réussit avec vos clients). Test peut prendre plusieurs formes, à partir de l’unité au niveau du développeur de test à la gestion des tests bêta dans une vaste gamme de matériel.


### <a name="test-on-all-platforms"></a>Sur toutes les plateformes de test

Il existe de légères différences entre les éléments .NET pris en charge sur Windows phone, Tablet PC et les appareils de bureau, ainsi que les limitations sur iOS qui empêchent le code dynamique doit être créé à la volée. Soit le plan de test du code sur plusieurs plateformes lorsque vous développez, ou planifiez de temps à refactoriser et mettre à jour la partie du modèle de votre application à la fin du projet.

Il est toujours recommandé d’utiliser l’émulateur/de simulateur pour tester plusieurs versions du système d’exploitation et également fonctionnalités/configurations de périphérique différent.

Vous devez également tester sur les périphériques de matériel physique différent autant que possible.


#### <a name="devices-in-cloud"></a>Appareils dans le cloud

L’écosystème de téléphone et tablette mobile est-elle tout le temps, ce qui empêche le tester sur le nombre de plus en plus de périphériques disponibles. Pour résoudre ce problème, un certain nombre de services offre la possibilité de contrôler à distance différents types d’appareils afin que les applications peuvent être installées et testées sans avoir à investir directement dans un grand nombre de matériel.

[Application Center Test](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest) offre un moyen simple pour tester des applications iOS et Android sur des centaines de différents appareils.


### <a name="test-management"></a>Gestion des tests

Lorsque vous testez des applications au sein de votre organisation ou de la gestion d’un programme bêta avec des utilisateurs externes, il existe deux défis :

-   **Distribution** : gérer le processus d’approvisionnement (en particulier pour les appareils iOS) et l’obtention de versions mises à jour de logiciels aux testeurs.
-   **Commentaires** – collecte des informations sur l’utilisation des applications et des informations détaillées sur les erreurs qui peuvent se produire.


Il existe un certain nombre de l’aide des services pour résoudre ces problèmes, grâce à une infrastructure intégrée dans votre application pour collecter et consigner les erreurs et d’utilisation et également ce qui simplifie le processus de déploiement pour vous aider d’abonnement et de gérer les testeurs et leurs appareils .

[Visual Studio Application Center](/appcenter/) offre une solution à ces problèmes, en fournissant la distribution de version test, rapport et informations d’utilisation des applications sophistiquées.



### <a name="test-automation"></a>Automatisation de test

Xamarin [UITest](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest) peut être utilisé pour créer des scripts de test qui peuvent être exécutés localement ou téléchargés sur l’interface utilisateur automatisés [Application Center Test](https://docs.microsoft.com/appcenter/test-cloud/).




## <a name="unit-testing"></a>Test unitaire



#### <a name="touchunit"></a>Touch.Unit

Xamarin.iOS inclut une infrastructure de test unitaire appelée Touch.Unit qui suit le style de JUnit/NUnit l’écriture des tests.

Reportez-vous à notre [tests unitaires avec Xamarin.iOS](~/ios/deploy-test/touch.unit.md) pour plus d’informations sur l’écriture des tests et Touch.Unit en cours d’exécution.



#### <a name="andrunit"></a>Andr.Unit

Il est équivalent de Touch.Unit pour Android appelé Andr.Unit open source. Vous pouvez le télécharger à partir de [github](https://github.com/spouliot/Andr.Unit) et en savoir plus sur l’outil sur [ @spouliotdu blog](http://spouliot.wordpress.com/2011/10/30/andr-unit-joins-the-family/).




## <a name="app-store-approvals"></a>Approbations de l’App Store

Apple et Microsoft fonctionnent le magasin uniquement sur leurs plateformes : l’App Store et un Marketplace respectivement. Les deux verrouiller leurs appareils et implémentent un processus de révision app rigoureux pour contrôler la qualité des applications disponibles en téléchargement. Nature d’ouverte d’Android signifie qu’il existe plusieurs options de magasin allant Play de Google, qui est largement disponible et n’a aucun processus de révision, à l’App Store d’Amazon pour Android et spécifiques au matériel efforts telles que des applications Samsung qui ont une plus limitée de distribution et implémenter un processus d’approbation.

En attente d’une application à réviser peut être très stressants - précieux signifie souvent les applications sont soumises pour approbation avec très peu marge d’erreur avant une date de lancement « cible ». Le processus peut prendre jusqu'à deux semaines et n’est pas nécessairement transparent : il existe des commentaires limité sur la progression de votre application jusqu'à ce qu’il est finalement rejetée ou approuvée. Rejet peut signifier manquante d’une fenêtre de marketing d’opportunité, surtout si elle se produit plusieurs fois, et passer des semaines entre votre date de lancement d’origine et lorsque l’application est enfin approuvée.



### <a name="be-prepared"></a>Soyez prêt

La première partie du Conseil : planifier le lancement de votre application à l’avance et prendre en considération un rejet possible et la nouvelle soumission. Chaque magasin vous oblige à créer un compte avant de soumettre votre application - cela dès que possible.
Lors de l’inscription de Google Play ne prend que quelques minutes si vos applications sont libres, le processus obtient beaucoup plus complexe si vous les vendez et que vous devez fournir des opérations bancaires et les informations fiscales. Apple et les processus de Microsoft sont à la fois beaucoup plus compliqué que de Google, il peut prendre une semaine ou plus pour obtenir votre compte approuvé donc contribuer instant vos plans de lancement.

Une fois votre compte a été approuvé, vous êtes prêt à envoyer une application. Le processus réel pour soumettre des applications est traité dans la documentation suivante :

-   [Publication sur l’App Store d’iOS d’Apple](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
-   [Préparation d’une application pour Google Play](~/android/deploy-test/publishing/publishing-to-google-play/index.md)
-  Les développeurs Windows rendez-vous sur le [centre de développement Windows](https://developer.microsoft.com/en-us/windows/windows-apps) pour en savoir plus sur l’envoi de leurs applications.


Le reste de cette section décrit les éléments à prendre en considération afin de garantir que l’approbation de votre application sans les interruptions.



### <a name="quality"></a>Qualité

Il semble évident, mais les applications seront refusées souvent, car elles ne répondent pas à un certain niveau de qualité : après tout, c’est la raison pour lesquelles les magasins organisés sont dans un processus d’approbation en premier lieu !

Incidents sont une raison courante de rejet. S’il est trop facile d’apporter des pannes de votre application, il a garanti être rejetée. Leurs applications dans l’attente que leur allez se bloquer, mais ils n’envoyez pas la plupart des développeurs. Testez votre application minutieusement avant de l’envoyer, en mettant l’accent ne pas simplement en veillant que tout fonctionne, mais également que vous avez gérer des scénarios d’erreur mobile courants tels que des problèmes de réseau et les contraintes de ressources telles que l’espace mémoire ou de stockage. Permet de tester le simulateur et les périphériques physiques - quelle que soit la manière dont le code s’exécute dans un simulateur, seul un appareil permettant d’expliquer les performances réelles d’une application. Utiliser les différents types d’appareils que vous pouvez trouver et inscrire une équipe testeurs de la version bêta, si vous le pouvez - services tiers peuvent aider à gérer les commentaires et la distribution bêta.

Tous les systèmes d’exploitation mobiles sera utilisée par une application qui ne commence pas assez rapidement. La longueur de période varie, mais en général les applications doivent visent à répondre en quelques secondes et utiliser des tâches en arrière-plan pour effectuer tout travail plus. Les applications qui prennent trop de temps à se charger ou ne sont pas suffisamment réactive en cours d’utilisation régulière est rejetées. Toujours fournir des commentaires des utilisateurs lorsque quelque chose qui se passe en arrière-plan, ou l’application s’affiche aux pannes et une fois encore, refusés.


### <a name="check-your-edge-cases"></a>Vérifiez vos cas de bord

Interruption commune pour les développeurs ne parvient pas à adresse-cas limites, en particulier ceux qui nécessitent la reconfiguration de leurs simulateur ou l’appareil pour tester correctement. Il est facile d’oublier que certains clients va « Autoriser » votre application pour accéder à leur emplacement, car une fois que le développeur a accepté la demande, ils êtes jamais invités à nouveau. L’utilisation du réseau et les autorisations sont occupés spécifiquement sur pendant le processus d’approbation, ce qui signifie que le rejet peut entraîner une petite supervision dans ces domaines.

La liste suivante est un bon point de départ pour la vérification de cas limites peuvent avoir été omis :

-   **Les clients peuvent « refuser » l’accès aux services** : en particulier dans iOS, l’accès aux données telles que les informations d’emplacement géographique ne sont disponible que si l’utilisateur accorde l’autorisation de votre application. Testeurs de l’application doivent fréquemment réinstaller l’application dans son état initial et n’autorisent pas les demandes d’autorisation pour vous assurer de que l’application se comporte de manière appropriée. Basculer l’autorisation sur et hors tension pour vérifier le comportement correct comme clients changent leurs avis.
-   **Les clients sont everywhere** – ne supposez pas qu’une application est utilisée uniquement dans la ville ou le pays dans lequel il a été développé ! Tout code qui fonctionne avec des coordonnées GPS, les valeurs de date et heure et les devises peut être affecté par l’emplacement et les paramètres régionaux du client. Toutes les plateformes offre un simulateur qui vous permettre de spécifier différents emplacements et paramètres régionaux - il permet de tester les emplacements dans l’hémisphère sud autres et avec les cultures mise en forme des dates et devises différemment. Valeurs de latitude et longitude peuvent être positif ou négatif, le séparateur de décimale peut être un point ou une virgule et dates peuvent être mise en forme une multitude de façons différentes - préoccuper !
-   **Lenteur des connexions réseau** – utilisez souvent des développeurs d’applications dans un monde idéal d’accélérée, toujours travailler connectivité réseau, ce qui évidemment n’est pas le cas dans le monde réel. Le test de connectivité réseau lent (par exemple, une mauvaise connexion 3 G), ainsi qu’avec aucun accès réseau est essentiel pour garantir qu'une application boguée n’expédition. Le processus d’approbation est toujours inclure un test avec l’appareil en mode avion, assurez-vous que vous avez testé que vous-même.
-   **Varie en fonction du matériel** : n’oubliez pas de test sur le matériel plus ancien, plus lent que vous envisagez de prendre en charge. Il existe deux aspects qui peuvent affecter votre application : performances, ce qui peuvent être inutilisable sur un dispositif plus anciens et la prise en charge des fonctionnalités matérielles telles qu’une caméra, microphone, GPS, gyroscope ou un autre composant facultatif. Les applications doivent se dégrader normalement (et pas incident) quand un composant est indisponible.




### <a name="guidelines-are-more-than-just-a-guide"></a>Les instructions sont plus qu’un guide « »

Apple est connue pour être strict quant au respect des instructions relatives à leur Interface humaine l’un des avantages clés de leur plate-forme est la cohérence (et l’augmentation perçue de facilité d’utilisation). Microsoft a adopté une approche similaire avec des applications Windows implémentant l’interface utilisateur de style Metro. Le processus d’approbation pour les deux plateformes exige votre application en cours d’évaluation pour le respect de la philosophie de conception.

Cela ne veut pas dire qu’innovation d’interface utilisateur n’est pas pris en charge ou à encourager, mais certains éléments vous « simplement ne doit pas faire » ou votre application est rejetée.

Sur iOS, cela inclut la mauvaise utilisation des icônes intégrés ou à l’aide d’autres métaphores bien établies de façon non cohérente ; par exemple, en utilisant l’icône « composer » pour que la création de contenu.

Les développeurs Windows doivent être de la même façon rigoureuse ; une erreur courante ne peut pas prendre en charge correctement le matériel précédent bouton en fonction des recommandations de Microsoft.

Encouragez vos concepteurs pour lire et suivre les instructions de conception pour chaque plateforme.



### <a name="implementing-platform-specific-features"></a>Implémenter les fonctionnalités spécifiques à la plateforme

Lorsqu’il s’agit de l’implémentation des services de plateforme spécifique, en particulier sur iOS, les choses sont un peu plus strictes. Pour éviter de rejet-automatique par Apple, il existe des règles à suivre avec les fonctionnalités d’iOS suivantes :

-   **Achats dans l’application** – les Applications ne doivent pas implémenter des mécanismes de paiement externe pour les produits numériques, y compris dans le jeu devise, fonctionnalités de l’application, abonnements magazines et beaucoup plus. les applications iOS doivent utiliser service iTunes d’Apple pour ce genre de fonctionnalités. Il existe une vulnérabilité - applications telles que le lecteur Kindle et certaines applications basées sur les abonnement vous permettent d’ailleurs le contenu qui est associé à un « compte », auquel vous pouvez ensuite accéder via l’application d’achat, toutefois dans ce cas l’application ne doit pas contenir des liens ou des références à la la sortie de l’application processus d’achat (ou une fois encore, il allez rejeté).
-   **la sauvegarde iCloud** – avec l’avènement des iCloud, les réviseurs d’Apple sont beaucoup plus strictes concernant l’applications permet de stockage (pour vérifier l’expérience du client de sauvegarde à distance est agréable). Les applications que l’espace de stockage en mesure de sauvegarde déchets peut obtenir rejetée, utilisent par conséquent, le dossier de Cache appropriée et suivez Apple d’autres recommandations liées au stockage.
-   **Kiosque** – journaux et magazines applications sont parfaitement adaptés à kiosque Apple, toutefois, les applications doivent implémenter au moins un renouvellement automatique abonnement et la prise en charge du téléchargement en arrière-plan pour être approuvées.
-   **Mappe** – il s’agit plus en plus courant pour ajouter des superpositions et autres fonctionnalités à maps mobiles, toutefois veiller à ne pas être d’altérer la carte 'crédits' informations (par exemple, le logo Google dans iOS5) comme cela entraînerait rejet.




### <a name="manage-your-metadata"></a>Gérer vos métadonnées

Outre les problèmes techniques évidents pouvant entraîner une application rejetée, il existe certaines plus subtiles aspects de votre contribution qui peut entraîner le rejet, notamment en ce qui les métadonnées (description, mots clés et les images marketing) que vous avez envoyer avec votre application pour l’affichage dans l’App Store ou le Marketplace.

-   **IMAGERIE** : suivez les instructions de la plateforme pour les icônes de l’application et stocker des images. N’utilisez pas avec des images, nous avons vu applications obtient rejetées, car leurs icônes proposées un dessin d’un iPhone !
-   **Marques** : Évitez d’utiliser des marques déposées différent du vôtre. Les applications ont été refusées pour mentionnant des marques dans la description de l’application ou même dans les mots clés App Store d’Apple.
-   **Description** – ne pas utiliser le mot « bêta » ou de quelque manière indiquent que l’application n’est pas prête pour les nombres premiers. Ne mentionnent pas les autres plateformes mobiles (même si votre application est inter-plateformes). Plus important encore, vérifiez que l’application est exactement ce que vous dites que c’est le cas. Si vous affichez un ensemble de fonctionnalités dans votre description, il sera évident comment utiliser chacune de ces fonctionnalités, ou vous obtiendrez un rejet « fonctionnalité mentionnée dans la description de l’application n’est pas implémentée ».


Placez autant d’efforts dans les métadonnées de l’application comme dans le développement et de test. Applications obtenir rejetées pour infractions mineures dans les métadonnées, il est utile de prendre le temps d’obtenir le droit.



### <a name="app-stores-not-for-everyone"></a>Magasins d’applications : Pas de tout le monde

Le principal objectif des magasins sur chaque plateforme est la distribution de consommateur - la possibilité d’atteindre des clients autant que possible. Cependant pas toutes les applications sont destinées à des consommateurs, il existe une croissance rapide base des applications internes et plus extranet qui nécessitent une distribution limitée aux employés, des fournisseurs ou des clients. Ces applications ne sont pas « pour la vente » et n’avez pas besoin de l’approbation, depuis la distribution de contrôles de développeur à un groupe d’utilisateurs de fermé.
Prise en charge pour ce type de déploiement varie selon la plate-forme.

Android offre la plus grande souplesse à cet égard : applications peuvent être installées directement à partir d’une URL ou une pièce jointe (tant que lui permet de configuration de l’appareil). Cela signifie qu’il est très facile à créer et distribuer des applications d’entreprise internes ou publier des applications sur des clients ou des fournisseurs.

Apple offre une option de déploiement en interne aux développeurs inscrits sur le site iOS Developer Enterprise Program, qui ignore le processus d’approbation App Store et permet aux entreprises de distribuer des applications en interne à leurs employés.
Malheureusement, cette licence ne traite pas la nécessité pour la distribution de l’application de type extranet à d’autres groupes fermés de clients ou de fournisseurs. [Entreprise (et Ad-Hoc) déploiement](~/ios/deploy-test/app-distribution/ipa-support.md)



### <a name="app-store-summary"></a>Résumé de l’App Store

Le processus de révision peut être décourageant, mais comme le reste du cycle de développement vous contribuez à garantir la réussite avec une planification et à l’attention aux détails. Tout se résume à quelques étapes simples : lire et comprendre les indications de l’interface utilisateur vous devez respecter pour respecter les règles si vous implémentez les fonctionnalités spécifiques à la plateforme, Testez minutieusement (puis tester d’autres) et enfin Assurez-vous que vos métadonnées de l’application est correct avant d’envoyer.

Un dernier mot des conseils aux développeurs de publication sur Google Play : l’absence de processus d’approbation peut sembler il facilite la tâche - mais vos clients seront encore plus élevés qu’une équipe de révision. Suivez ces instructions, car bien que votre application peut obtenir rejetée, sinon elle sera vos clients effectuant le rejet.

