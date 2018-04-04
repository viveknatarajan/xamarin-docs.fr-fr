---
title: Autres ressources
ms.prod: xamarin
ms.assetid: AE5A864E-192D-475E-C731-99249C2E7D9E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/13/2018
ms.openlocfilehash: f7833e238afe41d4a5ac7b8dde4c168f298752fc
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="alternate-resources"></a>Autres ressources

Ressources de remplacement sont les ressources qui ciblent un périphérique spécifique ou la configuration d’exécution telles que la langue actuelle, de la taille de l’écran en particulier ou de la densité en pixels. Si Android peut correspondre à une ressource qui est plus spécifique pour un périphérique particulier ou la configuration que la ressource par défaut, cette ressource sera utilisée à la place. S’il ne trouve pas une autre ressource qui correspond à la configuration actuelle, les ressources par défaut seront chargés. Comment Android décide quelles ressources seront utilisées par une application seront abordées plus en détail ci-dessous, dans la section emplacement de la ressource

Ressources de remplacement sont organisées sous la forme d’un sous-répertoire dans le dossier de ressources en fonction du type de ressource, tout comme les ressources par défaut. Le nom du sous-répertoire autre ressource se trouve dans le formulaire : _ResourceType_-_qualificateur_

*Qualificateur* est un nom qui identifie une configuration de périphérique spécifique.
Il peut être plus d’un qualificateur dans un nom de chacun d’eux séparés par un tiret. Par exemple, la capture d’écran ci-dessous montre un projet simple qui a des ressources pour différentes configurations telles que les paramètres régionaux, densité de l’écran, taille de l’écran et l’orientation de remplacement :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Autres ressources](alternate-resources-images/alternate-resources-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![Autres ressources](alternate-resources-images/alternate-resources-xs.png)
 
-----
 

Les règles suivantes s’appliquent lorsque vous ajoutez des qualificateurs à un type de ressource :

1. Il peut y avoir plus d’un qualificateur, avec chaque qualificateur séparé par un tiret.

2. Les qualificateurs peut être ne spécifiés qu’une seule fois.

3. Les qualificateurs doivent être dans l’ordre de qu'apparition dans le tableau ci-dessous.

Pour référence, les qualificateurs possibles sont répertoriées ci-dessous :

- **MMC et NMC** &ndash; le [régional mobile](http://en.wikipedia.org/wiki/List_of_mobile_country_codes) (MMC) et éventuellement le [code de réseau mobile](http://en.wikipedia.org/wiki/Mobile_Network_Code) (NMC). La carte SIM fournira la MMC, tandis que l’appareil est connecté au réseau fournira le NMC. Bien qu’il soit possible de paramètres régionaux de cible à l’aide de l’indicatif du pays mobiles, l’approche recommandée consiste à utiliser le qualificateur de langue spécifié ci-dessous. Par exemple, pour les ressources cibles en Allemagne, le qualificateur serait `mcc262`. Pour les ressources cibles de T-Mobile aux États-Unis, le qualificateur est `mcc310-mnc026`.
  Pour une liste complète des codes de pays mobile et réseau mobile consultez <http://mcc-mnc.com/>.

- **Langage** &ndash; à deux lettres [code ISO 639-1 langue](http://en.wikipedia.org/wiki/ISO_639-1) et, éventuellement suivies de deux lettres [code de région ISO 3166-alpha 2](http://en.wikipedia.org/wiki/ISO_3166-1_alpha-2). 
  Si les deux qualificateurs sont fournis, ils sont séparés par un `-r`. Par exemple, pour les paramètres régionaux de langue Français de cible, puis le qualificateur de `fr` est utilisé. Pour cibler des paramètres régionaux franco-canadiens, la `fr-rCA` serait utilisée. Pour obtenir une liste complète des codes de langue et région, consultez [Codes pour la représentation sous forme de noms de langues](http://www.loc.gov/standards/iso639-2/php/English_list.php) et [les éléments de code et les noms de pays](http://www.iso.org/iso/country_codes/iso_3166_code_lists/country_names_and_code_elements.htm).

- **Plus petite largeur** &ndash; spécifie la plus petite largeur d’écran sert à exécuter sur l’application. Couvert plus en détail dans [création de ressources pour les écrans de variable](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Disponible dans le niveau de l’API 13 (Android 3.2) et versions ultérieures. Par exemple, le qualificateur `sw320dp` est utilisé pour cibler des appareils dont hauteur et la largeur est au moins 320dp.

- **Largeur disponible** &ndash; la largeur minimale de l’écran dans le format w*N*dp, où *N* correspond à la largeur de la densité de pixels indépendants.
  Cette valeur peut changer que l’utilisateur fait pivoter l’appareil. Couvert plus en détail dans [création de ressources pour les écrans de variable](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Disponible dans le niveau de l’API 13 (Android 3.2) et versions ultérieures. Exemple : le qualificateur w720dp est utilisé pour cibler des appareils qui ont une largeur de 720dp minimum.

- **Hauteur disponible** &ndash; la hauteur minimale de l’écran dans le format h*N*dp, où *N* correspond à la hauteur de point de distribution. Cette valeur peut changer que l’utilisateur fait pivoter l’appareil. Couvert plus en détail dans [création de ressources pour les écrans de variable](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Disponible dans le niveau de l’API 13 (Android 3.2) et versions ultérieures. Par exemple, le qualificateur h720dp est utilisé pour cibler des appareils qui ont une hauteur de moins de 720dp

- **Taille de l’écran** &ndash; ce qualificateur est une généralisation de ces ressources sont pour la taille d’écran. Il est couvert en détail dans [création de ressources pour les écrans de variable](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Les valeurs possibles sont `small`, `normal`, `large` et `xlarge`. Ajouté au niveau de l’API 9 (Android 2.3/Android 2.3.1/Android 2.3.2)

- **Écran Aspect** &ndash; cette est basée sur les proportions, pas l’orientation de l’écran. Un écran longs est plus large. Ajouté dans le niveau de l’API 4 (Android 1.6). Les valeurs possibles sont longues et notlong.

- **L’Orientation de l’écran** &ndash; orientation Portrait ou paysage de l’écran. Cela peut changer pendant la durée de vie d’une application.
  Les valeurs possibles sont `port` et `land`.

- **Ancrer le Mode** &ndash; pour ancrer des périphériques d’une voiture ou à un bureau ancrer. Ajouté dans le niveau de l’API 8 (2.2.x Android). Les valeurs possibles sont `car` et `desk`.

- **Mode nuit** &ndash; ou non l’application est en cours d’exécution pendant la nuit ou dans la journée. Cela peut changer pendant la durée de vie d’une application et est destinée à permettre aux développeurs d’utiliser des versions plus sombre d’une interface pendant la nuit. Ajouté dans le niveau de l’API 8 (2.2.x Android). Les valeurs possibles sont `night` et `notnight`.

- **Densité de pixels (PPP) de l’écran** &ndash; le nombre de pixels dans une zone donnée sur l’écran physique. En général, exprimée en points par pouce (PPP). Les valeurs possibles sont :

    - `ldpi` &ndash; Écrans de faible densité.

    - `mdpi` &ndash; Écrans de densité moyenne

    - `hdpi` &ndash; Écrans haute densité

    - `xhdpi` &ndash; Écrans de très haute densité

    - `nodpi` &ndash; Ressources qui ne doivent ne pas être monté en charge

    - `tvdpi` &ndash; Introduite dans le niveau d’API 13 (Android 3.2) pour les écrans entre mdpi et hdpi.

- **Type d’écran tactile** &ndash; Spécifie le type d’écran tactile un périphérique a peut-être. Les valeurs possibles sont `notouch` (aucun écran tactile), `stylus` (résistance sinon adapté à un stylet), et `finger` (un écran tactile).

- **Disponibilité du clavier** &ndash; Spécifie le type du clavier n’est disponible. Cela peut changer pendant la durée de vie d’une application &ndash; par exemple lorsqu’un utilisateur ouvre le clavier physique. Les valeurs possibles sont :

    - `keysexposed` &ndash; L’appareil a un clavier. S’il n’existe aucun clavier logiciel activée, elle est uniquement utilisée lorsque le clavier physique est ouvert.

    - `keyshidden` &ndash; L’appareil n’est pas un clavier physique, mais il est masqué et aucun clavier logiciel n’est activé.

    - `keyssoft` &ndash; l’appareil a un clavier logiciel activé.

- **Méthode d’entrée de texte principale** &ndash; permet de spécifier quels types de clés de matériel sont disponibles pour l’entrée. Les valeurs possibles sont :

    - `nokeys` &ndash; Il n’existe pas de clé matérielle pour l’entrée.

    - `qwerty` &ndash; Un clavier AZERTY est disponible.

    - `12key` &ndash; Il existe un clavier matériel de clé de 12


- **Disponibilité de la clé navigation** &ndash; pour lorsque la navigation (pavé directionnel) 5 bidirectionnel ou boîtier est disponible. Cela peut changer pendant la durée de vie de votre application. Les valeurs possibles sont :

    - `navexposed` &ndash; les touches de navigation sont disponibles à l’utilisateur

    - `navhidden` &ndash; les touches de navigation ne sont pas disponibles.

-  **Méthode de Navigation principale Non tactile** &ndash; le type de navigation disponible sur l’appareil. Les valeurs possibles sont :

    - `nonav` &ndash; la fonctionnalité de navigation uniquement disponible est l’écran tactile

    - `dpad` &ndash; un boîtier (pavé directionnel) est disponible pour la navigation

    - `trackball` &ndash; le périphérique a une boule de commande pour la navigation

    - `wheel` &ndash; le scénario rare où il existe une ou plusieurs directionnelle roues disponibles

-  **Version de la plateforme (API niveau)** &ndash; au niveau de l’API prises en charge par le périphérique dans le format v*N*, où *N* est le niveau de l’API qui est ciblé. Par exemple, v11 cibleront un niveau 11 (Android 3.0) de l’API appareil.


Pour plus d’informations sur la ressource qualificateurs voir [fournissant des ressources](http://developer.android.com/guide/topics/resources/providing-resources.html) sur le site Web des développeurs Android.


## <a name="how-android-determines-what-resources-to-use"></a>Comment Android détermine les ressources à utiliser

Il est très possible et probable qu’une application Android contient de nombreuses ressources. Il est important de comprendre comment Android sélectionne les ressources pour une application lorsqu’elle s’exécute sur un appareil.

Android détermine les ressources de base en effectuant une itération sur le test suivant de règles :

- **Éliminer les qualificateurs contradictoires** &ndash; , par exemple, si l’orientation du périphérique est définie sur portrait, puis tous les répertoires de ressources paysage seront rejetées.

- **Ignorer les qualificateurs ne pas pris en charge** &ndash; pas tous les qualificateurs sont disponibles pour tous les niveaux d’API. Si un répertoire de ressources contient un qualificateur qui n’est pas pris en charge par l’appareil, ce répertoire des ressources sera ignoré.

- **Identifier le qualificateur de priorité la plus élevé suivant** &ndash; faisant référence à la table ci-dessus sélectionnez le qualificateur de priorité le plus élevé suivant (de haut en bas).

- **Conserver les répertoires de ressources pour le qualificateur** &ndash; s’il existe les répertoires de ressource qui correspondent à la table ci-dessus le qualificateur de sélectionner le qualificateur de priorité le plus élevé suivant (de haut en bas).

Ces règles sont également illustrées dans l’organigramme suivant :

[![Diagramme de flux de ressources](alternate-resources-images/flowchart-sml.png)](alternate-resources-images/flowchart.png#lightbox)

Lorsque le système recherche des ressources spécifiques à une densité et ne les trouvez pas, il va tenter de localiser les autres ressources spécifiques de densité et mettre à l’échelle. Android peut ne pas utiliser les ressources par défaut.
Par exemple, lors de la recherche d’une ressource de faible DENSITE et il n’est pas disponible, Android peut sélectionner version haute densitée de la ressource sur les ressources par défaut ou la densité moyenne. Pour cela, car la ressource à densité élevée et peut être réduite par un facteur de 0,5, ce qui entraîne moins de problèmes de visibilité à l’échelle d’une ressource de densité moyenne, ce qui nécessiterait un facteur de 0,75.

Par exemple, considérez une application qui a les répertoires de ressources drawable suivants :

    drawable
    drawable-en
    drawable-fr-rCA
    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi
    drawable-port-ldpi
    drawable-port-notouch-12key

Et maintenant, l’application est exécutée sur un appareil avec la configuration suivante :

- **Paramètres régionaux** &ndash; en-GB
- **Orientation** &ndash; port
- **Écran de densité** &ndash; hdpi
- **Type d’écran tactile** &ndash; notouch
- **Méthode d’entrée principal** &ndash; 12key

Pour commencer, les ressources en Français sont éliminés, car ils sont en conflit avec les paramètres régionaux de `en-GB`, ce qui laisse avec :

    drawable
    drawable-en
    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi
    drawable-port-ldpi
    drawable-port-notouch-12key

Ensuite, le premier qualificateur est sélectionné dans la table de qualificateurs ci-dessus : MMC et NMC. Il n’existe aucun des répertoires de ressources qui contiennent ce qualificateur du code MMC/NMC est donc ignoré.

Le qualificateur suivant est sélectionné, qui est la langue. Il existe des ressources qui correspondent au code de langue. Tous les répertoires de ressources qui ne correspondent pas le code de langue de `en` sont rejetés, afin que la liste des ressources est désormais :

    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi

Le qualificateur suivant présent est pour l’orientation de l’écran, par conséquent, tous les répertoires de ressources qui ne correspondent pas à l’orientation de l’écran de `port` sont éliminés :

    drawable-en-port
    drawable-en-port-ldpi

L’élément suivant est l’identificateur de la densité de l’écran, `ldpi`, ce qui entraîne l’exclusion d’un répertoire des ressources plus :

    drawable-en-port-ldpi

À la suite de ce processus, Android utilise les ressources drawable dans le répertoire de ressources `drawable-en-port-ldpi` pour le périphérique.

> [!NOTE]
> Les qualificateurs de taille d’écran fournissent une exception à ce processus de sélection. Il est possible pour Android sélectionner les ressources qui sont conçues pour un écran plus petit que le périphérique en cours. Par exemple, un appareil à écran large peut utiliser les ressources fournir pour un écran de taille normal. Cependant l’inverse de ce n’est pas vrai : le même périphérique grand écran n’utilisera pas les ressources fournies pour un écran xlarge. Si Android ne peut pas trouver un jeu de ressources qui correspond à une taille d’écran, l’application se bloque.
