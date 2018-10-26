---
title: Autres ressources
ms.prod: xamarin
ms.assetid: AE5A864E-192D-475E-C731-99249C2E7D9E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/13/2018
ms.openlocfilehash: 0384d96ddc96f8d0b16a42f691305f26ea25881d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108754"
---
# <a name="alternate-resources"></a>Autres ressources

Ressources de remplacement sont les ressources qui ciblent un appareil spécifique ou la configuration d’exécution telles que la langue actuelle, taille de l’écran particulier ou une densité de pixels. Si Android peut correspondre à une ressource qui est plus spécifique pour une configuration ou un périphérique particulier, celui de la ressource par défaut, cette ressource sera utilisée à la place. S’il ne trouve pas une autre ressource qui correspond à la configuration actuelle, les ressources par défaut seront chargés. Comment Android décide les quelles ressources seront utilisées par une application seront abordée plus en détail ci-dessous, dans la section emplacement de la ressource

Autres ressources sont organisées en tant que sous-répertoire dans le dossier de ressources en fonction du type de ressource, tout comme les ressources par défaut. Le nom du sous-répertoire autre ressource est sous la forme : _ResourceType_-_qualificateur_

*Qualificateur* est un nom qui identifie la configuration d’un appareil spécifique.
Il peut être plusieurs qualificateurs dans un nom, chacun d’eux séparés par un tiret. Par exemple, la capture d’écran ci-dessous montre un projet simple qui a d’autres ressources pour différentes configurations telles que les paramètres régionaux, la densité de l’écran, taille de l’écran et l’orientation :

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Autres ressources](alternate-resources-images/alternate-resources-vs.png)
 
# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

![Autres ressources](alternate-resources-images/alternate-resources-xs.png)
 
-----
 

Les règles suivantes s’appliquent lorsque vous ajoutez des qualificateurs à un type de ressource :

1. Il peut exister plusieurs qualificateurs, avec chaque qualificateur séparé par un tiret.

2. Les qualificateurs peut-être spécifiés une seule fois.

3. Qualificateurs doivent être dans l’ordre de qu'apparition dans le tableau ci-dessous.

Les qualificateurs possibles sont répertoriées ci-dessous pour référence :

- **MCC et NMC** &ndash; le [code pays mobile](http://en.wikipedia.org/wiki/List_of_mobile_country_codes) (MCC) et éventuellement le [code réseau mobile](http://en.wikipedia.org/wiki/Mobile_Network_Code) (NMC). La carte SIM fournira la MCC, tandis que l’appareil est connecté au réseau fournira le NMC. Bien qu’il soit possible de paramètres régionaux de cible à l’aide de l’indicatif du pays mobiles, l’approche recommandée consiste à utiliser le qualificateur de langue spécifié ci-dessous. Par exemple, pour les ressources cibles en Allemagne, le qualificateur serait `mcc262`. Pour les ressources cibles de T-Mobile dans le fuseau horaire, le qualificateur est `mcc310-mnc026`.
  Pour une liste complète des codes de pays mobiles et réseau mobile consultez <http://mcc-mnc.com/>.

- **Langage** &ndash; deux lettres [code ISO 639-1 langue](http://en.wikipedia.org/wiki/ISO_639-1) et éventuellement suivie de deux lettres [code de région ISO 3166-alpha-2](http://en.wikipedia.org/wiki/ISO_3166-1_alpha-2). 
  Si les deux qualificateurs sont fournis, ils sont séparés par un `-r`. Par exemple, pour les paramètres régionaux en parlant le Français de la cible, puis le qualificateur de `fr` est utilisé. Pour cibler les paramètres régionaux franco-canadiens, le `fr-rCA` serait utilisé. Pour obtenir une liste complète des codes de langue et région, consultez [Codes pour la représentation des noms de langues](http://www.loc.gov/standards/iso639-2/php/English_list.php) et [les éléments de code et les noms de pays](http://www.iso.org/iso/country_codes/iso_3166_code_lists/country_names_and_code_elements.htm).

- **Plus petite largeur** &ndash; spécifie la largeur d’écran plus petit vise à s’exécuter sur l’application. Couvert plus en détail dans [création de ressources pour faire varier les écrans](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Disponible dans le niveau d’API 13 (Android 3.2) et versions ultérieures. Par exemple, le qualificateur `sw320dp` est utilisé pour cibler des appareils dont la hauteur et la largeur est au moins 320dp.

- **Largeur disponible** &ndash; la largeur minimale de l’écran dans le format w*N*dp, où *N* correspond à la largeur de la densité (independent pixels).
  Cette valeur peut changer que l’utilisateur fait pivoter l’appareil. Couvert plus en détail dans [création de ressources pour faire varier les écrans](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Disponible dans le niveau d’API 13 (Android 3.2) et versions ultérieures. Exemple : le qualificateur w720dp est utilisé pour cibler des appareils qui ont une largeur d’au moins 720dp.

- **Hauteur disponible** &ndash; la hauteur minimale de l’écran dans les heures de format*N*dp, où *N* correspond à la hauteur de point de distribution. Cette valeur peut changer que l’utilisateur fait pivoter l’appareil. Couvert plus en détail dans [création de ressources pour faire varier les écrans](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Disponible dans le niveau d’API 13 (Android 3.2) et versions ultérieures. Par exemple, le qualificateur h720dp permet de cibler des appareils qui ont une hauteur de 720dp minimum

- **Taille de l’écran** &ndash; ce qualificateur est une généralisation de ces ressources sont pour la taille d’écran. Il est couvert plus en détail dans [création de ressources pour faire varier les écrans](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Les valeurs possibles sont `small`, `normal`, `large` et `xlarge`. Ajouté dans le niveau d’API 9 (Android 2.3/Android 2.3.1/Android 2.3.2)

- **Écran Aspect** &ndash; elle est basée sur le rapport hauteur / largeur, pas l’orientation de l’écran. Un écran non long est plus large. Ajouté dans le niveau d’API 4 (Android 1.6). Les valeurs possibles sont de longue durée et notlong.

- **L’Orientation d’écran** &ndash; orientation de l’écran Portrait ou paysage. Cela peut changer pendant la durée de vie d’une application.
  Les valeurs possibles sont `port` et `land`.

- **Mode d’ancrage** &ndash; pour ancrer des appareils dans une voiture ou un bureau d’ancrage. Ajouté dans le niveau d’API 8 (2.2.x Android). Les valeurs possibles sont `car` et `desk`.

- **Mode nuit** &ndash; ou non l’application est en cours d’exécution pendant la nuit ou dans la journée. Cela peut changer pendant la durée de vie d’une application et est destiné à permettre aux développeurs d’utiliser des versions plus sombre d’une interface pendant la nuit. Ajouté dans le niveau d’API 8 (2.2.x Android). Les valeurs possibles sont `night` et `notnight`.

- **Densité de pixels (PPP) de l’écran** &ndash; le nombre de pixels dans une zone donnée sur l’écran physique. En général, exprimée sous forme de points par pouce (PPP). Les valeurs possibles sont :

    - `ldpi` &ndash; Écrans de faible densité.

    - `mdpi` &ndash; Écrans de densité moyenne

    - `hdpi` &ndash; Écrans à haute densité

    - `xhdpi` &ndash; Écrans de très haute densité

    - `nodpi` &ndash; Ressources qui ne doivent ne pas être mis à l’échelle

    - `tvdpi` &ndash; Introduite dans le niveau d’API 13 (Android 3.2) pour les écrans entre mdpi et hdpi.

- **Type d’écran tactile** &ndash; Spécifie le type d’écran tactile un appareil peut avoir. Les valeurs possibles sont `notouch` (sans écran tactile), `stylus` (résistant écran tactile adapté à un stylet), et `finger` (un écran tactile).

- **Disponibilité du clavier** &ndash; spécifie quel type de clavier est disponible. Cela peut changer pendant la durée de vie d’une application &ndash; par exemple lorsqu’un utilisateur ouvre le clavier physique. Les valeurs possibles sont :

    - `keysexposed` &ndash; L’appareil a un clavier disponible. S’il n’existe aucun clavier logiciel activé, puis il est utilisé uniquement lorsque le clavier physique est ouvert.

    - `keyshidden` &ndash; L’appareil dispose d’un clavier matériel, mais il est masqué et aucun clavier logiciel n’est activé.

    - `keyssoft` &ndash; l’appareil a un clavier logiciel activé.

- **Méthode d’entrée de texte principale** &ndash; permet de spécifier quels types de clés de matériel sont disponibles pour l’entrée. Les valeurs possibles sont :

    - `nokeys` &ndash; Il n’existe aucune clé matérielle pour l’entrée.

    - `qwerty` &ndash; Un clavier AZERTY est disponible.

    - `12key` &ndash; Il existe un clavier matériel de clé de 12


- **Disponibilité de clé de navigation** &ndash; lorsque 5 voies ou le pavé directionnel navigation (pavé directionnel) est disponible. Cela peut changer pendant la durée de vie de votre application. Les valeurs possibles sont :

    - `navexposed` &ndash; les touches de navigation sont disponibles à l’utilisateur

    - `navhidden` &ndash; les touches de navigation ne sont pas disponibles.

-  **Méthode de Navigation principale Non-Touch** &ndash; le type de navigation disponible sur l’appareil. Les valeurs possibles sont :

    - `nonav` &ndash; la fonctionnalité de navigation uniquement disponible est l’écran tactile

    - `dpad` &ndash; un pavé directionnel (pavé directionnel) est disponible pour la navigation

    - `trackball` &ndash; l’appareil a un trackball pour la navigation

    - `wheel` &ndash; le scénario rare où il existe une ou plusieurs directionnelles roues disponibles

-  **Version de la plateforme (niveau API)** &ndash; au niveau de l’API pris en charge par l’appareil dans le format v*N*, où *N* est le niveau d’API ciblé. Par exemple, v11 ciblera niveau 11 (Android 3.0) de l’API appareil.


Pour plus d’informations sur la ressource qualificateurs consultez [fournissant des ressources](http://developer.android.com/guide/topics/resources/providing-resources.html) sur le site Web Android aux développeurs.


## <a name="how-android-determines-what-resources-to-use"></a>Comment Android détermine les ressources à utiliser

Il est très possible et probable qu’une application Android contient de nombreuses ressources. Il est important de comprendre comment Android sélectionnera les ressources pour une application lorsqu’elle s’exécute sur un appareil.

Android détermine les ressources de base en effectuant une itération sur le test suivant de règles :

- **Éliminer les qualificateurs contradictoires** &ndash; , par exemple, si l’orientation du périphérique est portrait, puis tous les répertoires de ressources paysage seront rejetées.

- **Ignorer les qualificateurs ne pas pris en charge** &ndash; pas tous les qualificateurs sont disponibles pour tous les niveaux d’API. Si un répertoire de ressources contient un qualificateur qui n’est pas pris en charge par l’appareil, ce répertoire de ressources sera ignoré.

- **Identifier le qualificateur de priorité la plus élevé suivant** &ndash; faisant référence à la table ci-dessus sélectionnez le qualificateur de priorité le plus élevé suivant (de haut en bas).

- **Conserver les répertoires de ressources pour le qualificateur** &ndash; si tous les répertoires de ressources qui correspondent au qualificateur pour le tableau ci-dessus n’y sélectionner le qualificateur de priorité le plus élevé suivant (de haut en bas).

Ces règles sont également illustrées dans l’organigramme suivant :

[![Diagramme de flux de ressources](alternate-resources-images/flowchart-sml.png)](alternate-resources-images/flowchart.png#lightbox)

Lorsque le système recherche des ressources spécifiques à la densité et ne les trouvez pas, il tente de localiser d’autres ressources spécifiques de densité et mettez-les à l’échelle. Android peut ne pas utiliser les ressources par défaut.
Par exemple, lorsque vous recherchez une ressource de faible DENSITE et il n’est pas disponible, Android peut sélectionner version haute densitée de la ressource sur les ressources par défaut ou densité moyenne. Pour cela, car la ressource à haute densité peut être réduite par un facteur de 0,5, ce qui entraîne moins de problèmes de visibilité que la mise à l’échelle vers le bas d’une ressource de densité moyenne, ce qui nécessiterait un facteur de 0,75.

Par exemple, considérez une application qui a les répertoires de ressources drawable suivants :

    drawable
    drawable-en
    drawable-fr-rCA
    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi
    drawable-port-ldpi
    drawable-port-notouch-12key

Et maintenant l’application est exécutée sur un appareil avec la configuration suivante :

- **Paramètres régionaux** &ndash; en-GB
- **Orientation** &ndash; port
- **Écran de densité** &ndash; hdpi
- **Type d’écran tactile** &ndash; notouch
- **Méthode d’entrée principal** &ndash; 12key

Pour commencer, les ressources Français sont éliminés comme ils sont en conflit avec les paramètres régionaux de `en-GB`, ce qui laisse avec :

    drawable
    drawable-en
    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi
    drawable-port-ldpi
    drawable-port-notouch-12key

Ensuite, le qualificateur premier est sélectionné dans le tableau de qualificateurs ci-dessus : MCC et NMC. Il n’existe aucun répertoire de ressources qui contiennent ce qualificateur du code MCC/NMC est donc ignoré.

Le qualificateur suivant est sélectionné, ce qui est la langue. Il existe des ressources qui correspondent au code de langue. Tous les répertoires de ressources qui ne correspondent pas le code de langue `en` sont rejetés, afin que la liste des ressources est désormais :

    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi

Le qualificateur suivant qui est présent est pour l’orientation de l’écran, afin que tous les répertoires de ressources qui ne correspondent pas à l’orientation d’écran de `port` sont éliminés :

    drawable-en-port
    drawable-en-port-ldpi

Ensuite vient le qualificateur de densité de l’écran, `ldpi`, ce qui entraîne l’exclusion d’un répertoire de ressources plus :

    drawable-en-port-ldpi

À la suite de ce processus, Android utilise les ressources drawable dans le répertoire de ressources `drawable-en-port-ldpi` pour l’appareil.

> [!NOTE]
> Les qualificateurs de taille d’écran fournissent une exception à ce processus de sélection. Il est possible pour Android sélectionner les ressources qui sont conçus pour un écran plus petit que fournit de l’appareil en cours. Par exemple, un appareil grand écran peut utiliser les ressources fournissent pour un écran de taille normal. Toutefois l’inverse de ce n’est pas vrai : le même appareil grand écran n’utilisera pas les ressources fournies pour un écran très grand. Si Android ne peut pas trouver un ensemble de ressources qui correspond à une taille d’écran donné, l’application se bloque.
