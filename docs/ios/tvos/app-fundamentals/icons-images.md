---
title: Utilisation des icônes et des Images
description: Cet article décrit la conception et l’utilisation des icônes et des images à l’intérieur d’une application Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: A2DA4347-0563-4C72-A8D7-5B9DE9E28712
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: c888ecf3d7e0f21734f2b89176eed56bf778dbf9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-icons-and-images"></a>Utilisation des icônes et des Images

_Cet article décrit la conception et l’utilisation des icônes et des images à l’intérieur d’une application Xamarin.tvOS._

Création captivantes icônes et les images sont un élément essentiel du développement d’une expérience utilisateur réalistes pour vos applications Apple TV. Ce guide couvre les étapes requises pour créer et inclure les éléments de graphique nécessaires pour vos applications Xamarin.tvOS :

- [Lancer l’Image](#Launch-Image) -une image de démarrage s’affiche lorsque votre application est démarrée pour la première et il est remplacée par le premier écran de l’application issue de l’exécution.
- [Couches d’Images](#Layered-Images) - spécifiques pour Apple TV, nouveau travail de couches des Images d’Apple avec l’effet parallaxe pour créer un effet 3D pour les éléments sélectionnés. Il existe plusieurs façons de [créer des Images en couche](#Creating-Layered-Images).
- [Icône de l’application](#App-Icons) -les icônes sont obligatoires pour non seulement l’Apple TV accueil de l’écran, mais pour l’App Store. Elles doivent être fournies en tant qu’une Image en couche.
- [Haut de l’étagère Image](#Top-Shelf-Image) -si votre application est placée sur la ligne du haut de l’écran d’accueil, vous devez une Image de l’étagère haut pour mettre en évidence les fonctionnalités de votre application. Si vous le souhaitez, vous pouvez fournir [le contenu dynamique en haut de l’étagère](#Dynamic-Top-Shelf-Content) pour mettre en surbrillance le contenu de votre application.
- [Images de centre de jeu](#Game-Center-Images) -si votre application est un jeu et utilise Game Center, plusieurs images supplémentaires sera nécessaire.
- [Définir les Images de projet Xamarin.tvOS](#Setting-Xamarin.tvOS-Project-Images) -couvre les étapes nécessaires pour définir l’Image de lancement et l’icône de l’application pour votre application Xamarin.tvOS.

> [!IMPORTANT]
> Toutes les images sur l’Apple TV sont à la résolution de 1 x (`@1x`) et vous ne devez _uniquement_ utiliser des images de cette taille. Y compris le plus grand, graphiques d’une résolution plus élevée non seulement de temps télécharger et utiliser plus de mémoire et de stockage, mais ils doivent être mise dynamiquement à l’échelle lors de l’exécution et seront affecter négativement les performances de dessin.

<a name="Launch-Image" />

## <a name="launch-image"></a>Lancer l’Image

L’Image de lancement est la première chose qui s’affiche lorsque votre application Xamarin.tvOS est initialement démarrée sur l’Apple TV, et par conséquent, chaque application tvOS doit fournir une Image de lancement. 

L’Image de lancement s’affiche rapidement et donne l’impression que votre application est rapide et réactives. L’Apple TV remplace l’Image de lancer le premier écran de votre application peu de temps il après.

Images de démarrage ne sont pas une opportunité d’annonces ou une expression artistique, ils existent uniquement pour donner l’impression que votre application lance rapidement et est prête à utiliser.

|Lancez la taille de l’Image|Notes|
|---|---|
|1920x1080px|Uniquement les fichiers .png de couches non|

Apple rend les suggestions suivantes pour la conception d’Image de lancement de votre application :

- **Presque identique au premier écran** -votre écran de lancement doit être aussi proche de votre écran de l’application première que possible. Y compris les différents graphiques ou un élément peut entraîner un agaçantes « flash » lorsque le premier écran s’affiche.
- **Éviter à l’aide de texte** -lancez les Images sont statiques et ne pourront donc pas être localisées avant d’être affichées.
- **Se détourner lancement** -les utilisateurs car Apple TV commutateur fréquemment des applications, vous ne devez pas attirer l’attention sur le processus de lancement d’application.
- **Aucun publicités ou la personnalisation** -lancement de l’Image ne doit pas être utilisé comme un écran à propos ou inclure toute personnalisation, sauf si elle est la partie statique du premier écran de votre application. Annonces sont strictement interdite.

<a name="Setting-the-Launch-Image" />

### <a name="setting-the-launch-image"></a>Définition de l’Image de démarrage

Pour définir l’Image de lancement de votre projet de tvOS, procédez comme suit :

1. Dans le **l’Explorateur de solutions**, double-cliquez sur `Assets.xcassets` à ouvrir pour le modifier : 

    [![](icons-images-images/asset01.png "Le fichier Assets.xcassets")](icons-images-images/asset01.png#lightbox)
2. Dans le **Asset éditeur**, cliquez sur le `LaunchImages` actif : 

    [![](icons-images-images/asset02.png "L’élément multimédia LaunchImages")](icons-images-images/asset02.png#lightbox)
3. Cliquez sur le **1 x Apple TV** entrée et sélectionnez l’Image de lancer ou faites éventuellement glisser une nouvelle image le système de fichiers : 

    [![](icons-images-images/asset03.png "Sélectionnez une Image de démarrage")](icons-images-images/asset03.png#lightbox)
4. Enregistrez les modifications apportées.

<a name="Layered-Images" />

## <a name="layered-images"></a>Images en couche

Nouveau pour Apple TV, travail superposées des Images avec l’effet parallaxe pour produire un effet 3D qui permet de conserver l’utilisateur sur le canapé mentalement connecté au contenu à l’écran dans la salle.

Contiennent des Images en couches à partir de deux (2) à cinq (5) séparer les couches sont combinées pour former une image complète. À l’exception de la couche d’arrière-plan, chaque couche utilise son ordre de plan, ainsi que la transparence pour créer une illusion de profondeur. Quand l’utilisateur interagit avec une Image en couches, les couches supérieures ordonné Z sont mis à l’échelle et superposées pour créer cet effet.

[![](icons-images-images/layered01.png "Diagramme d’Images Z ordonné superposé")](icons-images-images/layered01.png#lightbox)

> [!IMPORTANT]
> Images en couches sont requis pour les icônes de votre application et sont facultatives pour les autres [éléments peut être actif](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) (par exemple, l’Image de l’étagère haut). Toutefois, Apple suggère l’utilisation d’Images de couches pour toute image qui peut obtenir le focus dans votre application.




Apple rend les suggestions suivantes pour concevoir vos Images en couche :

- **Rendre le Opaque de couche d’arrière-plan** -votre couche d’arrière-plan (couche 1) **doit** être opaques ou que vous obtenez une erreur lorsque vous essayez d’utiliser l’Image en couche sur Apple TV. Toutes les autres couches peuvent contenir plusieurs niveaux de transparence pour améliorer l’effet 3D.
- **Isoler de premier plan, de milieu et d’éléments de l’arrière-plan** -placer les principaux éléments (par exemple, le jeu de caractères) au premier plan, utilisez au milieu d’éléments secondaires ou des ombres. Enfin, inclure un arrière-plan neutre pour fournir une étape pour vos couches supérieures.
- **Conserver le texte au premier plan** -sauf si vous souhaitez que votre texte pour être masquées par les niveaux supérieurs, en général il doit se trouver sur la couche de niveau supérieur.
- **Utilisez la superposition Simple** -l’effet parallaxe a été conçu pour être subtiles, gardez vos couches à un nombre minimal afin d’éviter des effets glacial irréalistes.
- **Inclure une Zone Safe** -étant donné que les couches supérieures peuvent être rognées pendant un effet parallaxe, vous devez créer une bordure de Zone de sécurité dans chaque couche. Si vous obtenez votre contenu trop fermer le bord de couches, il peut devenir retiré. Couches supérieures seront confrontés à plus de mise à l’échelle et de rognage à des couches inférieures. Consultez le [dimensionnement de couches d’images](#Sizing-Image-Layers) section ci-dessous.
- **Afficher un aperçu de souvent** -Images de couches doit afficher un aperçu souvent pour vous assurer que l’effet 3D souhaité se produit et qu’aucun du contenu sur des calques individuels est en cours rognée. Vous devez afficher un aperçu de vos Images en couche sur du vrai matériel Apple TV pour vous assurer de que leur rendu comme prévu.

Chaque fois que possible, vous devez toujours utiliser la fonction intégrée `UIKit` contrôles pour afficher vos Images en couche, car elles obtiennent automatiquement de l’effet parallaxe lorsqu’ils sont dans le focus.

<a name="Sizing-Image-Layers" />

### <a name="sizing-image-layers"></a>Dimensionnement de couches d’images

Il est important de se rappeler à inclure un _Safe Zone_ bordure dans chaque couche qui constituera votre Image en couche. Étant donné que les couches individuelles peuvent être mis à l’échelle et rognées durant l’effet parallaxe, le contenu des couches peut retiré si elle est trop près bord de la couche :

[![](icons-images-images/layered02.png "bordure de 35 pixels")](icons-images-images/layered02.png#lightbox)

<a name="Creating-Layered-Images" />

### <a name="creating-layered-images"></a>Création de couches des Images

il fonctionne avec des Images en couche tvOS dans les formats suivants :

- **Fichiers de voiture** -il s’agit d’un format propriétaire de catalogue créé par Apple. Vous ne créez pas directement les fichiers de voiture, ils sont créés au moment de la compilation à partir de tous les fichiers LSR et inclus dans votre offre groupée.
- **Les Images LSR** -il s’agit d’un format d’image propriétaires créé par Apple. Utilisez le [parallaxe exportateur Adobe Photoshop plug-in](https://itunespartner.apple.com/assets/downloads/ParallaxExporter_Apps.zip) ou [parallaxe aperçu](http://itunespartner.apple.com/assets/downloads/Parallax%20Previewer.dmg) pour créer et afficher un aperçu des couches des Images au format LSR.
- **Assets.xcassets** - à partir de deux (2) à cinq (5) standard `.png` images mis en forme inclus dans un catalogue qui sera compilé dans une voiture ou un LSR mis en forme en couche une Image au moment de la compilation.
- **Les fichiers LCR** -il s’agit d’un format de fichier propriétaire créé par Apple. Fichiers LCR sont destinés à être utilisés en tant que contenu supplémentaire téléchargé à partir d’un de vos serveurs de contenu. Fichier de réplication continue locale n’a jamais doit être inclus dans votre offre groupée. LCR sont générés à partir de fichiers LSR ou Photoshop à l’aide du `layerutil` outil de ligne de commande inclus dans Xcode.

<a name="The-Parallax-Previewer" />

### <a name="the-parallax-previewer"></a>Le Générateur d’aperçu parallaxe

Apple créé le [parallaxe aperçu](http://itunespartner.apple.com/assets/downloads/Parallax%20Previewer.dmg) pour afficher un aperçu et des Images créées en couche requis pour les icônes de l’application et des éléments facultatifs peut être actif. Le Générateur d’aperçu montre chaque couche constituant l’Image en couche terminée :

[![](icons-images-images/layered03.png "Le Générateur d’aperçu parallaxe")](icons-images-images/layered03.png#lightbox)

Lors de l’aperçu d’une Image en couches, vous pouvez utiliser la souris pour faire pivoter l’image et afficher un aperçu de l’effet parallaxe. Utilisez le **+** (plus) et **-** (moins) de boutons permettant d’ajouter et supprimer des couches.

Lorsque vous créez une nouvelle Image en couche, peuvent être exportée au format LSR et du groupe de votre application.

Pour plus d’informations sur la création et l’aperçu des Images de couches, consultez le site d’Apple [création d’une illustration de parallaxe](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/CreatingParallaxArtwork.html#//apple_ref/doc/uid/TP40015241-CH19-SW1) section de la [Guide de programmation d’application de tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/).

<a name="App-Icons" />

## <a name="app-icons"></a>Icônes de l’application

Votre application Xamarin.tvOS requiert non seulement une icône d’application pour l’écran d’accueil de Apple TV, mais également une icône pour le magasin d’applications. L’icône d’application est le votre premier modifier pour effectuer une impression excellente sur les utilisateurs potentiels et doit communiquer l’objectif de votre application en un coup de œil.

[![](icons-images-images/icon01.png "L’icône d’application")](icons-images-images/icon01.png#lightbox)

Chaque application doit fournir un petit et une version de grande taille de son icône de l’application. La petite icône servira à l’écran d’accueil de Apple TV lorsque l’application est installée. La version de grande taille est utilisée par l’App Store. L’icône d’application volumineux doit reproduire l’apparence de la version de la petite icône.

|Petite icône||Grandes icônes||
|---|---|---|---|
|Taille réelle|400x240px|Size|1280x768px|
|Taille de la Zone de sécurité|370x222px|||
|Taille sans focus|300x180px|||
|Taille ayant le focus|370x222px|||

> [!IMPORTANT]
> Icônes de l’application doit être fourni en tant que **en couche des Images**. Consultez le [en couche une Image](#Layered-Images) section ci-dessus pour plus d’informations.




Apple offre des suggestions suivantes pour créer des icônes de votre application :

- **Fournir un Point de Focus unique** – conception votre icône avec un point de focus unique placé directement dans le centre de l’icône.
- **Utilisez un arrière-plan Simple** – simplifier votre arrière-plan de l’icône afin que les couches supérieures ressortent. Envisagez d’utiliser une couleur simple ou un dégradé subtil.
- **Limiter la quantité de texte** – étant donné que le nom de l’application s’affiche sous l’icône lorsqu’il est sélectionné par l’utilisateur, vous devez inclure uniquement de texte lorsqu’il est essentiel de la conception de l’icône.
- **N’utilisez pas des captures d’écran** – captures d’écran sont trop complexes pour une icône et ne pas autoriser l’utilisateur de voir l’objectif de votre application en un coup de œil.
- **Conserver icônes carré** – tvOS applique automatiquement un masque légèrement arrondit les coins de vos icônes. N’incluez pas cet arrondi vous-même.
- **Séparer les couches votre soigneusement** – texte doit être dans le coin supérieur couche la plupart des éléments secondaires dans le milieu et un arrière-plan neutre et permet à vos couches supérieures se traduisent.
- **Utilisez les dégradés et les ombres soigneusement** – dégradés et des ombres peuvent entrer en conflit avec l’effet parallaxe afin qu’ils doivent être utilisées avec précaution. Simple haut en bas, styles de dégradé de clair à sombre fonctionnent le mieux. Shadows normalement mieux en tant que teintes aigus, et.
- **Faire varier la transparence des couches** : utilisation de différents niveaux de transparence sur les niveaux supérieurs de l’icône de l’application pour augmenter l’effet 3D. La couche d’arrière-plan doit être opaque, ou il génère une erreur.

<a name="Setting-the-App-Icons" />

### <a name="setting-the-app-icons"></a>Définir les icônes d’application

Pour définir les icônes d’application requis pour votre projet de tvOS, procédez comme suit :

1. Dans le **l’Explorateur de solutions**, double-cliquez sur `Assets.xcassets` à ouvrir pour le modifier : 

    [![](icons-images-images/asset01.png "Le fileg Assets.xcassets")](icons-images-images/asset01.png#lightbox)
2. Dans le **Asset éditeur**, développez le `App Icon & Top Shelf Image` actif : 

    [![](icons-images-images/asset04.png "Développez l’élément multimédia en haut de l’étagère Image")](icons-images-images/asset04.png#lightbox)
3. Ensuite, développez le `App Icon - Small` actif : 

    [![](icons-images-images/asset05.png "Développez l’icône d’application - petite actif")](icons-images-images/asset05.png#lightbox)
4. Puis développez le `Back` actif, puis cliquez sur le `Contents` entrée : 

    [![](icons-images-images/asset06.png "Puis développez l’élément multimédia précédent")](icons-images-images/asset06.png#lightbox)
5. Cliquez sur le **1 x entrée d’Apple TV** et sélectionnez un fichier image.
6. Répétez les étapes ci-dessus pour le `Front` et `Middle` actifs.
7. Puis répétez la même procédure pour définir le `App Icon - Large` actif.
4. Enregistrez les modifications apportées.

<a name="Top-Shelf-Image" />

## <a name="top-shelf-image"></a>Image du haut de l’étagère

Si l’utilisateur a placé votre application Xamarin.tvOS sur la ligne en haut de l’écran d’accueil de Apple TV, une grande Image de l’étagère haut s’affichera lorsque votre application sera sélectionnée par l’utilisateur. Cette image doit mettre en évidence les fonctionnalités de votre application ou fournir des liens directs vers son contenu.

[![](icons-images-images/topshelf01.png "Exemple de l’étagère Image supérieure")](icons-images-images/topshelf01.png#lightbox)

L’Image de l’étagère Top peut être fourni en tant que statique unique `.png` ou `.lsr` fichier (voir [création d’Images en couche](#Creating-Layered-Images)) ou il peut être créé dynamiquement lors de l’exécution en tant qu’une seule ligne d’éléments peut être actif (consultez [ Le contenu dynamique haut de l’étagère](#Dynamic-Top-Shelf-Content) ci-dessous).

|Taille de l’Image haut de l’étagère|Notes|
|---|---|
|1920x720px|.Png statique ou un fichier de .lsr en couche|

Apple offre des suggestions suivantes pour la création d’Images de votre tablette haut :

- **Utiliser des images statiques de riches** – si votre application ne fournit pas un contenu dynamique, son Image de l’étagère haut sera non actif. Utiliser cette image pour mettre en évidence les fonctionnalités de l’application ou votre image de marque.
- **Lien vers du contenu de l’application** – dynamique en haut de l’étagère dotent un lien rapide vers le contenu que votre utilisateur trouve plus importante dans votre application. Utilisez cette zone pour fournir un lien rapide pour démarrer votre application et directement dans le contenu donné.
- **Exposer le contenu le plus récent** – contenu de riches en haut de l’étagère peut dessiner un utilisateur dans votre application et les rendre plus son utilisation. Utiliser cette zone pour illustrer la plus élevé nominal ou plus récent le contenu.
- **Contenu personnalisé** – place les utilisateurs les plus fréquemment utilisées ou applications favorites dans la ligne du haut de l’écran d’accueil. Utilisez l’interface de haut pour afficher le contenu, qu'ils seraient intéressés.
- **Annonces interdit** – annonces sont strictement interdits s’affiche dans le haut de l’étagère. Vous pouvez afficher le contenu le plus récent être achetée, mais aucune information de tarification ne doit être affichée.

### <a name="setting-the-top-shelf-image"></a>Définition de l’Image du haut de l’étagère

Pour définir l’Image de l’étagère haut requis pour votre projet de tvOS, procédez comme suit :

1. Dans le **l’Explorateur de solutions**, double-cliquez sur `Assets.xcassets` à ouvrir pour le modifier : 

    [![](icons-images-images/asset01.png "Le fichier Assets.xcassets")](icons-images-images/asset01.png#lightbox)
2. Dans le **Asset éditeur**, développez le `App Icon & Top Shelf Image` actif : 

    [![](icons-images-images/asset04.png "Développez l’élément multimédia en haut de l’étagère Image")](icons-images-images/asset04.png#lightbox)
3. Cliquez sur le `Top Shelf Image` actif : 

    [![](icons-images-images/asset07.png "L’élément multimédia en haut de l’étagère Image")](icons-images-images/asset07.png#lightbox)
5. Cliquez sur le **1 x entrée d’Apple TV** et sélectionnez un fichier image.
6. Enregistrez les modifications apportées.

<a name="Dynamic-Top-Shelf-Content" />

### <a name="dynamic-top-shelf-content"></a>Contenu dynamique haut de l’étagère

Au lieu d’afficher en haut de l’étagère Image statique, la conservation de haut peut contenir une ligne dynamique de [éléments peut être actif](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) ou un ensemble dynamique de défilement bannières. Deux ces style dynamique vous permettent de mettre en surbrillance le contenu fourni par votre application ou d’un saut dans ses fonctionnalités utilisées le plus.

<a name="Sectioned-Content-Row" />

#### <a name="sectioned-content-row"></a>Ligne de contenu coupée

Ce type de contenu dynamique en haut de l’étagère présente une seule ligne de défilement, les éléments peut être actif éventuellement divisé en sections. Il est généralement utilisé pour mettre en surbrillance, Favoris ou que vous avez récemment consulté le contenu de l’application.

Le contenu se présente comme une unique liste défilement horizontale du contenu avec une étiquette qui apparaissent sous l’élément actuel de contenu sélectionné (qui possède actuellement le focus). Si l’utilisateur sélectionne un élément de contenu donné, votre application doit être lancée, et ils doivent être prises directement dans ce contenu.

Les tailles de contenu suivant sera nécessaire :

||Affiches (2:3)|Carré (1:1)|HDTV (16:9)|
|---|---|---|---|
|Taille réelle|404x608px|608x608px|908x512px|
|Taille de la Zone de sécurité|380x570px|570x570px|852x479px|
|Taille sans focus|333x500px|500x500px|782x440px|
|Taille ayant le focus|380x570px|570x570px|852x479px|

Apple offre des suggestions suivantes pour la ligne Sectioned contenu :

- **Terminer la ligne** – vous devez fournir assez de contenu pour couvrir la largeur totale de l’écran.
- **Mise à l’échelle des Images mixtes** – contenu Sectioned la ligne a été conçu pour contenir une combinaison de tailles d’image (à partir de la liste ci-dessus). Si vous ne devez combiner les tailles d’image Toutefois, sachez que la mise à l’échelle supplémentaire s’appliquera pour normaliser l’affichage de contenu.

<a name="Scrolling-Inset-Banners" />

#### <a name="scrolling-inset-banners"></a>Défilement incrustation bannières

Si vous le souhaitez, votre application Xamarin.tvOS peut présenter son contenu dans les rayons de haut en tant qu’automatiquement le défilement et bouclage de collection de bannières presque plein écran. Ce style est généralement utilisé pour présenter un nouveau contenu comme nouveau émissions de télévision.

En plus de la barre de défilement automatique, l’utilisateur peut prendre le contrôle des bannières et faites défiler dans les deux sens à l’aide de l’élément distant Siri. Effectue un petit, mouvement circulaire sur Siri distant lorsqu’une bannière est activé s’activent l’effet parallaxe pour que la bannière.

**Image de bannière (très large)**

|   |   |
|---|---|
|Taille réelle|1940x624px|
|Taille de la Zone de sécurité|1740x620px|
|Taille sans focus|1740x560px|
|Taille ayant le focus|1740x620px|

Le défilement incrustation bannières peut soit être fournie comme statique `.png` ou en couche `.lsr` fichier.

Apple offre des suggestions suivantes pour les bannières incrustation de défilement :

- **Quantité de contenu** -vous devez fournir un minimum de trois (3) les bannières pour le défilement pour semble naturel. Vous devez inclure les bannières pas plus de huit (8) ou elle rendre navigation difficile pour l’utilisateur final.
- **Contenu de texte** : Si votre bannière requiert le texte doit être inclus dans l’image de bannière. Si vous utilisez des Images de couches, votre texte doit être dans la couche de niveau supérieur.

Consultez d’Apple [TVServices Framework référence](https://developer.apple.com/library/prerelease/tvos/documentation/TVServices/Reference/TVServices_Ref/index.html#//apple_ref/doc/uid/TP40016412) pour plus d’informations sur l’ajout d’une Extension de l’étagère haut à votre application pour fournir un contenu dynamique en haut de l’étagère.

<a name="Game-Center-Images" />

## <a name="game-center-images"></a>Images Game Center

Si votre application Xamarin.tvOS est un jeu et que vous avez ajouté la prise en charge Game Center, plusieurs ressources d’image plus seront nécessaires :

- **Icônes de réalisation** -une image opaque est requise pour chaque réalisation sera rognée automatiquement dans un cercle. Primes sont des éléments non actif.
- **Illustration du tableau de bord** -image facultative peut être fournie qui apparaît en haut du tableau de bord de votre application dans le Game Center. Ces images sont non actif.
- **Classement des illustrations** -vous devez fournir entre un (1) à trois (3) les images de proportions 16:9 pour chaque classement des qui prend en charge de votre application. Il peut s’agir soit statique `.png` ou en couche `.lsr` fichiers. La classement des illustrations sont peut être actif.

||Icônes de réalisation|Illustration du tableau de bord|Classement des illustrations|
|---|---|---|---|
|Taille visible|200x200px|923x150px|N/A|
|Taille réelle|320x320px|N/A|659x371px|
|Taille de la Zone de sécurité|N/A|N/A|618x348px|
|Taille sans focus|N/A|N/A|548x309px|
|Taille ayant le focus|N/A|N/A|618x348px|

Pour plus d’informations sur l’utilisation de Game Center, consultez le site d’Apple [Guide de programmation Game Center](https://developer.apple.com/library/prerelease/tvos/documentation/NetworkingInternet/Conceptual/GameKit_Guide/Introduction/Introduction.html).

<a name="Working-with-Images" />

## <a name="working-with-images"></a>Utilisation d’images

Étant donné que tvOS 9 est un sous-ensemble d’iOS 9, les mêmes techniques utilisées pour inclure et afficher des images dans une application Xamarin.iOS, fonctionnent également pour une application Xamarin.tvOS. Veuillez consulter notre [affichage d’une Image](~/ios/app-fundamentals/images-icons/displaying-an-image.md) documentation pour plus d’informations.

<a name="Setting-Xamarin.tvOS-Project-Images" />

## <a name="setting-xamarintvos-project-images"></a>Définir les Images de Xamarin.tvOS projet

Comme indiqué ci-dessus, toutes les applications tvOS nécessitent un [lancer une Image](#Launch-Image), et [icône de l’application](#App-Icons). Cette section aborde la sélection de l’Image de lancement et l’icône de l’application pour votre projet d’application Xamarin.tvOS une fois qu’ils ont été définies dans un catalogue.

Effectuez ce qui suit :

1. Dans le **l’Explorateur de solutions**, double-cliquez sur le `Info.plist` à ouvrir pour le modifier : 

    [![](icons-images-images/info01.png "Le fichier Info.plist")](icons-images-images/info01.png#lightbox)
2. Dans le **Info.Plist éditeur**, sélectionnez le catalogue de ressources (configuré ci-dessus dans le [définissant les icônes d’application](#Setting-the-App-Icons) section) pour le **icônes de l’application**: 

    [![](icons-images-images/info02.png "L’éditeur de fichier Info.Plist.")](icons-images-images/info02.png#lightbox)
3. Ensuite, sélectionnez le catalogue de ressources (configuré ci-dessus dans le [définition de l’Image de lancement](#Setting-the-Launch-Image) section) pour le **lancer les Images**.
4. Enregistrez les modifications apportées.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a couvert tous les types d’images et tailles utilisés dans une application Xamarin.tvOS. Tout d’abord, il couvert lancer les Images, les Images en couche, icônes de l’application, en haut de l’étagère Images et Game Center Images. Puis il couvert d’utilisation des images dans votre application Xamarin.tvOS.

## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS les Guides Interface humaine](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application de tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
