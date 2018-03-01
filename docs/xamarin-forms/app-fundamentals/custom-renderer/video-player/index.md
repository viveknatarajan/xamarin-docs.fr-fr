---
title: "Implémentation d’un lecteur vidéo"
ms.topic: article
ms.prod: xamarin
ms.assetid: CE9E955D-A9AC-4019-A5D7-6390D80DECA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: e818bc3fa9793f093c10ac2617c5a822d08213d4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="implementing-a-video-player"></a>Implémentation d’un lecteur vidéo

Il est parfois souhaitable de lire des fichiers vidéos dans une application de Xamarin.Forms. Cette série d’articles explique comment écrire des convertisseurs personnalisés pour iOS, Android et la plateforme Windows universelle (UWP) pour une classe Xamarin.Forms nommée `VideoPlayer`.

Dans le [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) exemple, tous les fichiers qui implémentent et prennent en charge `VideoPlayer` sont dans un dossier nommé `FormsVideoLibrary` et identifié par l’espace de noms `FormsVideoLibrary` ou espaces de noms qui commencent `FormsVideoLibrary`. Cette organisation et d’affectation de noms doit faciliter copier les fichiers de lecteur vidéo dans votre propre solution Xamarin.Forms.

`VideoPlayer` peut lire des fichiers vidéo à partir de trois types de sources :

- À l’aide d’une URL Internet
- Une ressource incorporée dans l’application de plateforme
- Vidéothèque du périphérique

Exiger des lecteurs vidéo *contrôles de transport*, qui sont des boutons de lecture et de la suspension de la vidéo et un positionnement de la barre qui indique la progression dans la vidéo et permet à l’utilisateur passer rapidement à un autre emplacement. `VideoPlayer` peut utiliser des contrôles de transport et barre de positionnement fournie par la plate-forme (comme indiqué ci-dessous), ou vous pouvez fournir des contrôles de transport personnalisé et une barre de positionnement. Voici le programme en cours d’exécution sous iOS, Android et la plateforme Windows universelle :

[![Lire une vidéo de Web](web-videos-images/playwebvideo-small.png "lire une vidéo de Web")](web-videos-images/playwebvideo-large.png "lire la vidéo sur le Web")

Bien entendu, vous pouvez activer le téléphone sur le côté pour agrandir la vue.

Un lecteur vidéo plus sophistiqué aurait certaines fonctionnalités supplémentaires, telles que le contrôle du volume et un mécanisme pour interrompre la vidéo lorsqu’un appel téléphonique est fourni un moyen de conserver l’écran actif pendant la lecture.

La série d’articles suivante montre progressivement la façon dont les convertisseurs de plate-forme et les classes de prise en charge sont générés :

## <a name="creating-the-platform-video-playersplayer-creationmd"></a>[Créez les lecteurs vidéo de plateforme](player-creation.md)

Chaque plate-forme requiert un `VideoPlayerRenderer` classe qui crée et gère un contrôle de lecteur vidéo pris en charge par la plateforme. Cet article explique la structure du convertisseur de classes et la façon dont les lecteurs sont créés.

## <a name="playing-a-web-videoweb-videosmd"></a>[Lecture d’une vidéo du Web](web-videos.md)

La plus courante de vidéos pour un lecteur vidéo est probablement Internet. Cet article décrit comment une vidéo sur le Web peut être référencé et utilisé en tant que source pour le lecteur vidéo.

## <a name="binding-video-sources-to-the-playersource-bindingsmd"></a>[Liaison de sources vidéo du lecteur](source-bindings.md)

Cet article utilise un `ListView` pour présenter une collection de vidéos à lire. Un programme montre comment le fichier code-behind peut définir la source vidéo du lecteur vidéo, mais un second programme montre comment vous pouvez utiliser les données de liaison entre le `ListView` et le lecteur vidéo.

## <a name="loading-application-resource-videosloading-resourcesmd"></a>[Chargement de vidéos de ressources d’application](loading-resources.md)

Vidéos peuvent être incorporés en tant que ressources dans les projets de plateforme. Cet article explique comment stocker ces ressources et les charger ultérieurement dans le programme pour être lu par le lecteur vidéo.

## <a name="accessing-the-devices-video-libraryaccessing-librarymd"></a>[L’accès à la vidéothèque du périphérique](accessing-library.md)

Lorsqu’une vidéo est créée à l’aide photo de l’appareil, le fichier vidéo est stocké dans la bibliothèque d’images de l’appareil. Cet article explique comment accéder au sélecteur d’image de l’appareil pour sélectionner la vidéo et puis lire à l’aide du lecteur vidéo.

## <a name="custom-video-transport-controlscustom-transportmd"></a>[Contrôles de transport vidéo personnalisée](custom-transport.md)

Bien que les lecteurs vidéo sur chaque plateforme fournissent leurs propres contrôles de transport sous la forme de boutons pour **lire** et **Pause**, vous pouvez supprimer l’affichage de ces boutons et fournir votre propre. Cet article vous montre comment.

## <a name="custom-video-positioningcustom-positioningmd"></a>[Positionnement personnalisé de vidéo](custom-positioning.md)

Chacun des lecteurs vidéo plateforme possède une barre de position qui affiche la progression de la vidéo et vous permet d’ignorer en avance ou en retour vers un emplacement particulier. Cet article explique comment vous pouvez remplacer cette barre position avec un contrôle personnalisé.





## <a name="related-links"></a>Liens associés

- [Démonstrations de lecteur vidéo (exemple)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
