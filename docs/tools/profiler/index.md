---
title: Xamarin Profiler
description: Ce guide présente les principales fonctionnalités du profileur Xamarin. Il consulter les profileurs, profilage et lorsqu’elles doivent être utilisées et à un flux de travail standard pour le profilage d’applications de Xamarin.
ms.prod: xamarin
ms.assetid: 3247fcee-6acc-470d-ab87-c1c511d67363
author: topgenorth
ms.author: toopge
ms.date: 06/03/2018
ms.openlocfilehash: 8882cb9cd84940e12865a730f75e36ecbaf9b6f0
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066674"
---
# <a name="xamarin-profiler"></a>Xamarin Profiler

_Ce guide présente les principales fonctionnalités du profileur Xamarin. Il consulter les profileurs, profilage et lorsqu’elles doivent être utilisées et à un flux de travail standard pour le profilage d’applications de Xamarin._

Réussite de l’application dépend de l’expérience utilisateur. En tant que développeur vous avez peut-être implémenté certaines fonctionnalités vraiment impressionnant dans votre application, mais si l’application est lente ou complète de pannes, l’utilisateur sera probablement la supprimer.

Historiquement, Mono a proposé à un profileur de ligne de commande puissant pour collecter des informations sur les programmes s’exécutant dans le runtime Mono appelé le [profiler de journal Mono](http://www.mono-project.com/docs/debug+profile/profile/profiler/). Xamarin Profiler une interface graphique pour le Générateur de profils de journal Mono et prend en charge le profilage Android, iOS, tvOS et les applications Mac sur Mac et Android, iOS et tvOS des applications sur Windows.

Le profileur Xamarin comportant des instruments disponibles pour le profilage, Allocations, les Cycles et Générateur de profils de temps. Ce guide explore que ces instruments mesurent et comment ils analysent votre application et clarifie la signification des données présentées à l’écran.

Ce guide examine les scénarios courants de profilage et présente le profileur en tant qu’outil pour analyser et optimiser des applications iOS et Android.

## <a name="download-and-install"></a>Téléchargez et installez

> [!NOTE]
> Vous devez être un [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/compare/) abonné pour déverrouiller cette fonctionnalité soit Visual Studio Enterprise sur Windows ou de Visual Studio pour Mac sur un Mac.

Xamarin Profiler est une application autonome et qu’il est intégré à Visual Studio pour Mac et Visual Studio activer le profilage à partir de l’IDE.

Télécharger le package d’installation pour votre plateforme :

- [**macOS**](https://dl.xamarin.com/profiler/profiler-mac.pkg)
- [**Windows**](https://dl.xamarin.com/profiler/profiler-windows.msi)

Une fois téléchargé, lancez le programme d’installation pour ajouter le profileur Xamarin à votre système.

## <a name="profilers-and-profiling"></a>Profileurs et profilage

Le profilage est une étape importante et souvent négligée en matière de développement d’applications. Le profilage est une forme d'**analyse dynamique de programme** - il analyse le programme alors qu’il est en cours d’exécution et en cours d’utilisation. Un profileur est un outil d’exploration de données qui collecte des informations sur la complexité temporelle, l’utilisation de méthodes particulières et la mémoire allouée. Un profileur permet d’explorer en profondeur et d'analyser ces mesures pour identifier les zones à problème dans le code.

Lors de la conception et développement d’une application, il est important de ne pas optimiser prématurément ; Autrement dit, passer du temps à développer votre code dans des zones qui est rarement accessible. Il s’agit de la puissance de profilage. Un profileur fournit l’éclairage les plus couramment utilisées les parties de votre base de code, et vous aide à localiser les zones où vous devez prendre les améliorations d’heure. Les développeurs doivent veiller à comprendre où la plupart du temps est consacré à votre application, et l’utilisation de la mémoire par votre application.

Le profilage est utile dans tous les types de développement, mais il est particulièrement important dans le développement mobile. Code non optimisé est beaucoup plus notable sur des plates-formes mobiles que sur les ordinateurs de bureau, et la réussite de votre application dépend de code et attrayantes et optimisé qui s’exécute de manière efficace.

## <a name="xamarin-profiler"></a>Xamarin Profiler

Le profileur Xamarin fournit aux développeurs un moyen pour profiler des applications à partir de Visual Studio pour Mac ou Visual Studio. Le profileur collecte et affiche des informations sur l’application, qui peut ensuite être utilisée par le développeur pour analyser le comportement d’une application. Il existe plusieurs façons pour profiler une application avec le profileur Xamarin, à savoir le profilage de mémoire et échantillonnage statistique. Elles sont effectuées via les Allocations et le Générateur de profils de temps instrumente respectivement.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Actuellement, le profileur Xamarin peut être utilisé pour tester des applications Xamarin.iOS, Xamarin.Android et Xamarin.Mac sur Mac (Via Visual Studio pour Mac). Le profileur est un processus distinct à partir de l’IDE, et par conséquent, en plus de l’exécution à partir de Visual Studio pour Mac, il peut être utilisé comme une application autonome pour examiner .exe et `.mlpd` les fichiers qui ont été produits à partir de la [profiler de journal mono](http://www.mono-project.com/docs/debug+profile/profile/profiler/).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Actuellement, le profileur Xamarin peut être utilisé pour tester des applications Xamarin.Android sur Windows (via Visual Studio et Visual Studio pour Mac). Le profileur est un processus distinct à partir de l’IDE, et par conséquent, en plus de l’exécution à partir de Visual Studio, il peut être utilisé comme une application autonome pour examiner .exe et `.mlpd` les fichiers qui ont été produits à partir de la [profiler de journal mono](http://www.mono-project.com/docs/debug+profile/profile/profiler/) .

-----

<a name="Profiler_Support" />

## <a name="profiler-support"></a>Prise en charge du Générateur de profils

Prise en charge de Xamarin Profiler est disponible sur les plateformes suivantes :

 - Visual Studio pour Mac (macOS, avec une licence d’entreprise)
    - Android
        - Appareil et l’émulateur
    - iOS
        - Appareil et le simulateur
    - tvOS (Instrument de temps n’est pas pris en charge)
        - Appareil et le simulateur
    - Mac

 - Visual Studio (uniquement **Enterprise** Version)
    - Android
        - Appareil et l’émulateur
    - iOS [expérimental]
        - Appareil et le simulateur
    - tvOS
        - Appareil et le simulateur

Notez que vous pouvez **uniquement** profil **déboguer** configurations.

## <a name="profiler-basics"></a>Principes de base du profileur

Cette section présente les différentes parties du profileur Xamarin et voyages ses fonctionnalités.

### <a name="allow-profiling-in-your-app"></a>Autoriser le profilage dans votre application

Avant que vous pouvez profiler avec succès de votre application, vous devez autoriser le profilage dans les Options de l’application du projet.

 - iOS :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

  **Générer > iOS débogage > activer le profilage**

  ![](images/ios-options-mac.png "iOS boîte de dialogue Options dans Visual Studio pour Mac")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  **Propriétés > Build iOS > activer le profilage**

  ![](images/ios-project-options-vs.png "iOS boîte de dialogue Options dans Visual Studio")

-----

 - Android :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

  **Générer > débogage Android > permettre l’Instrumentation de développeur**

  ![Boîte de dialogue Options Android dans Visual Studio pour Mac](images/android-project-options.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  **Générer > débogage Android > permettre l’Instrumentation de développeur**

  ![Boîte de dialogue Options Android dans Visual Studio pour Mac](images/android-project-options-vs-sml.png)

-----

### <a name="launching-the-profiler"></a>Lancement du profileur

Le profileur Xamarin peut être lancé à partir de votre IDE lorsque vous profilez votre application Android ou e/s, ou comme une application autonome.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

#### <a name="launching-from-visual-studio-for-mac"></a>Lancement à partir de Visual Studio pour Mac

1. Tout d’abord, assurez-vous que vous avez votre application chargée dans Visual Studio pour Mac et sélectionnez la configuration de débogage (par défaut).
2. Accédez à **exécuter > Démarrer le profilage**dans Visual Studio pour Mac, ou **Analyze > Xamarin Profiler** dans Visual Studio, pour ouvrir le Générateur de profils, comme illustré dans le diagramme ci-dessous :

  ![](images/start-profiling-xs.png "Lancement du profileur à partir de Visual Studio pour Mac")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

#### <a name="launching-from-visual-studio"></a>Lancement à partir de Visual Studio

1.  Tout d’abord, assurez-vous que vous disposez de votre application chargée dans Visual Studio, sélectionnez la configuration de débogage (par défaut), comme indiqué ci-dessus.
2.  Accédez à **Analyze > Xamarin Profiler** dans Visual Studio, pour ouvrir le Générateur de profils, comme illustré dans le diagramme ci-dessous :

![Lancement du profileur à partir de Visual Studio](images/start-profiling-vs.png)

-----

Si les éléments de menu ne s’affichent pas, reportez-vous à la [guide de dépannage](~/tools/profiler/troubleshooting.md).

Cela lance le Générateur de profils et démarre automatiquement le profilage de l’application.

Le profileur peut servir à mesurer les performances et la mémoire. Pour cela, via les Allocations et le Générateur de profils de temps instruments, nous allons examiner en détail dans la section suivante.

#### <a name="saving-and-loading-profiler-sessions"></a>Enregistrement et chargement de Sessions du profileur

Pour enregistrer une session de profilage à tout moment, choisissez **fichier > Enregistrer sous...**  à partir de la barre de menus du profileur. Ceci permet d’enregistrer le fichier dans _mlpd_ format, un format fortement compressé spéciale pour le profilage des données.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Une fois qu’il a été installé Xamarin Profiler sont accessibles dans votre dossier d’Applications comme illustré dans la capture d’écran ci-dessous :

![](images/applications.png "Ouvrez autonome du Générateur de profils de Mac")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Une fois qu’il a été installé l’application Xamarin Profiler sont accessibles dans le répertoire de votre Application :

![](images/applications-vs.png "Ouvrez autonome du Générateur de profils à partir de Windows ")

-----

Vous pouvez charger *.mlpd* des fichiers dans le profileur en ouvrant l’application autonome, en sélectionnant **choisir la cible** et du chargement du fichier.

Pour plus d’informations, consultez [génération des fichiers de .mlpd ](~/tools/profiler/troubleshooting.md#gen_mlpd).

## <a name="profiler-features"></a>Fonctionnalités du Générateur de profils

Xamarin Profiler est composé de cinq sections, comme illustré ci-dessous :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![](images/profiler-mac-sml.png "Sections du Générateur de profils dans Visual Studio pour Mac")](images/profiler-mac.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](images/profiler-vs.png "Sections du Générateur de profils dans Visual Studio")](images/profiler-vs.png#lightbox)

-----

- **Barre d’outils** – situé en haut du profileur, il offre les options pour démarrer ou arrêter le profilage, sélectionnez un processus cible, afficher le temps d’exécution de l’application et sélectionnez les affichages fractionnés qui composent l’application du Générateur de profils.
- **Instrumenter liste** – répertorie tous les instruments chargés pour la session de profilage.
- **Tracer le graphique** – ces graphiques concernent horizontalement les instruments dans la liste d’instrumentation. Un curseur (affiché sous le Générateur de profils de temps) peut servir à modifier l’échelle.
- **Instrumenter zone détails** -contient les données affichées par la vue sélectionnée de l’appareil en cours. Nous allons nous intéresser à ces vues plus en détail dans la section ci-dessous.
- **Affichage de l’inspecteur** – contient les sections qui peuvent être sélectionnées par le contrôle segmenté. Les sections sont dépendantes de l’appareil sélectionné et inclut : paramètres de Configuration, les statistiques, les informations de Trace de la pile et chemin d’accès à la racine.

### <a name="allocations"></a>Allocations

Les informations relatives aux allocations fournissent des informations détaillées sur les objets dans l’application lorsqu'ils sont en cours de création et sur le garbage collector.

En haut du profileur est les allocations qui affiche la quantité de mémoire allouée à intervalles réguliers pendant le profilage. Actuellement, le graphique d’allocations est le nombre total d’allocations et non la taille du segment de mémoire à ce stade dans le temps. Dans un sens, il s’arrêtera jamais, seulement jamais augmentera. Cela inclut les objets alloués sur la pile. Selon la version du runtime utilisée, le graphique peut être différent, même pour la même application.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![](images/allocations1.png "Informations relatives aux allocations")](images/allocations1.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](images/allocations1-vs.png "Informations relatives aux allocations")](images/allocations1-vs.png#lightbox)

-----

Il existe différentes vues de données dans l’acte d’Allocations, qui permettent aux développeurs d’analyser la manière dont leur application est à l’aide et la libération de mémoire. Ces vues sont décrites ci-dessous :

 -   **Allocations** – Cela affiche une liste de toutes les allocations et les regroupe par nom de classe. Cela fournit une excellente présentation des classes et méthodes utilisés, la fréquence à laquelle ils sont utilisés et la taille collective des classes utilisées. Double-clic sur une classe affiche le la mémoire allouée : 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

  [![](images/allocations3.png "Onglet d’allocations")](images/allocations3.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/allocations2-vs.png "Onglet d’allocations")](images/allocations2-vs.png#lightbox)

-----

La vue de l’inspecteur pour les Allocations fournit les options de filtrage et regroupement d’objets, en fournissant des statistiques sur la mémoire allouée, ainsi que les allocations supérieures, ainsi que les vues pour la Trace de la pile et le chemin d’accès à la racine.

 -   **Arborescence des appels** – Cela affiche l’arborescence des appels entière de tous les threads dans l’application et inclut des informations sur la mémoire allouée sur chaque nœud. Lorsqu’un élément est sélectionné dans la liste, tous les nœuds frères apparaîtront gris. Vous pouvez développer l’arborescence ou double-cliquez sur l’élément à descendre dans celui-ci. Lors de l’affichage de cette vue de données, l’inspecteur de paramètres d’affichage permet de modifier la façon dont elles sont présentées. Il existe actuellement deux options :
    1.  **Arborescence des appels d’inversé** – il considère que la trace de pile de haut en bas. C’est une option de vue pratique qu’elle indique les méthodes le plus profond où le processeur a consacré sa durée.
    2.  **Séparé par thread** – cette option organise l’arborescence des appels par thread.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

  [![](images/allocations2.png "Onglet d’arborescence des appels")](images/allocations2.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/allocations3-vs.png "Onglet d’arborescence des appels")](images/allocations3-vs.png#lightbox)

-----

 -   **Instantanés** – ce volet affiche des informations sur les instantanés de la mémoire. Pour générer ces lorsque le profilage d’une application en temps réel, cliquez sur le _caméra_ bouton dans la barre d’outils à chaque point que vous aimeriez voir quelle mémoire est conservée et publié. Vous pouvez ensuite cliquer sur chaque capture instantanée pour découvrir ce qui se passe sous le capot. Notez que les instantanés ne peuvent être réalisées qu’en direct de profilage d’une application. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

  [![](images/allocations4.png "Onglet de captures instantanées")](images/allocations4.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/allocations4-vs.png "Onglet de captures instantanées")](images/allocations4-vs.png#lightbox)

-----

### <a name="time-profiler"></a>Générateur de profils de temps

L’acte de temps Profiler mesure exactement combien de temps est consacré à chaque méthode d’une application. L’application est suspendue à intervalles réguliers, une trace de pile est exécutée sur chaque thread actif. Chaque ligne dans la zone de détail d’instrumenter indique le chemin d’exécution qui a été suivi.

Le graphique, comme illustré dans la capture d’écran ci-dessous, affiche le nombre d’échantillons reçus par l’application en cours d’exécution :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Heure du profileur Instrument](images/time1.png)](images/time1.png#lightbox) 

[![Heure du profileur Instrument : liste d’exemples](images/time3.png)](images/time3.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Heure du profileur Instrument](images/time1-vs.png)](images/time1-vs.png#lightbox) 

[![Heure du profileur Instrument : liste d’exemples](images/time3-vs.png)](images/time3-vs.png#lightbox) 

-----

- **Arborescence des appels** – montre durée de chaque méthode :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

  [![](images/time2.png "Heure du profileur Instrument – arborescence des appels")](images/time2.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/time2-vs.png "Heure du profileur Instrument – arborescence des appels")](images/time2-vs.png#lightbox) 

-----

### <a name="cycles"></a>Cycles

Grâce à l’utilisation de code c# et F # gérés, il peut être assez courant et malheureusement relativement facile de créer des références aux objets qui ne sont jamais supprimés. Cet instrument vous permet de repérer les objets et d’afficher les cycles référencés dans votre application.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Les cycles de pré-instrumentation](images/cycles.m751-sml.png)](images/cycles.m751.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Les cycles de pré-instrumentation](images/cycles-vs-sml.png)](images/cycles-vs.png#lightbox) 

-----

## <a name="profiling-applications"></a>Profilage d’Applications

Actuellement, seuls les configurations de débogage par défaut peuvent être profilées.

Si vous profilez une application avec toute autre configuration, la boîte de dialogue de message suivant s’affiche :


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Boîte de dialogue Erreur de profilage](images/image001.png)](images/image001.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](images/image1vs.png "Boîte de dialogue Erreur de profilage")](images/image1vs.png#lightbox) 

-----

Sélectionnez **mise à jour** pour continuer.

### <a name="sgen-garbage-collector-and-profiling"></a>SGen Garbage Collector et profilage

Le [SGen](http://www.mono-project.com/docs/advanced/garbage-collector/sgen/) RÉCUPÉRATEUR de mémoire est utilisé pour toutes les plateformes de Xamarin.

SGen est un forme canonique dans le catalogue global, qui alloue des objets d’une application en trois segments, reproduction, segment de mémoire principale et l’espace d’objet volumineux. Cela permet une exécution plus rapide de garbage collection. SGen est actuellement le GC par défaut de Xamarin.Android et les applications de Xamarin.iOS unifiée.

Application Xamarin.iOS à l’aide de l’API classique utilisé le GC Boehm – classique, non générations du garbage collector. Comme il est classique, il est moins probable libérer de la mémoire disponible, ce qui peut entraîner des résultats inexacts lorsque vous utilisez le profileur. Pour cette raison, les informations relatives aux Allocations ne peut pas être utilisé avec le garbage collector de Boehm.

Lorsque vous êtes invité à une boîte de dialogue si votre application utilise le garbage collector Boehm, Xamarin ne recommande pas de basculement d’application iOS existante qui utilisent Boehm à SGen sans recherche prudent et un test approfondi. Xamarin également ne pas est recommandé de basculer vers SGen pour le profilage et du retour, car ces résultats ne fournira pas précis bancs d’essai d’utilisation de la mémoire.

Pour plus d’informations sur la gestion de la mémoire, reportez-vous à la [mémoire et les performances les meilleures pratiques](~/cross-platform/deploy-test/memory-perf-best-practices.md) guide.

## <a name="summary"></a>Récapitulatif

Dans ce guide, nous avons à ce que le profilage est et comment il est avantageux pour le développeur. Ensuite, nous avons introduit le profileur Xamarin, certaines informations dans son fonctionnement et l’historique des. Enfin, nous fait pouvait les fonctionnalités de Xamarin Profiler et exploré les Allocations et les Instruments du Générateur de profils de temps.

## <a name="related-links"></a>Liens associés

- [Mémoire et les performances les meilleures pratiques](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Notes de publication](https://developer.xamarin.com/releases/profiler/preview/)
