---
title: Images et des icônes
description: Cette section inclut une variété d’articles qui couvrent l’utilisation des images dans une application Xamarin.iOS, telles que leur utilisation sous forme d’icônes, de lancer des écrans ou les inclure dans les contrôles et en fournissant des icônes pour les types de document personnalisées.
ms.prod: xamarin
ms.assetid: 0AB8CC07-11E4-0D75-4119-AED1A1252424
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: fd191c898d5bb015d2d394d42db1049bb0128fb7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="images-and-icons"></a>Images et des icônes

_Cette section inclut une variété d’articles qui couvrent l’utilisation des images dans une application Xamarin.iOS, telles que leur utilisation sous forme d’icônes, de lancer des écrans ou les inclure dans les contrôles et en fournissant des icônes pour les types de document personnalisées._

Il existe plusieurs façons de cette image actifs sont utilisés à l’intérieur d’une application iOS. À partir de simplement afficher une image dans le cadre de l’interface utilisateur d’une application, assigner à un contrôle d’interface utilisateur comme un `UIButton` ou `UIImageView`, à la fourniture des icônes et écrans de démarrage, Xamarin.iOS permet de facilement ajouter la grande illustration d’une application iOS comme suit : 

- **Résolution des Images indépendantes** – utiliser d’iOS intégrée pour utiliser des images de résolutions de différents appareils et types (iPhone, iPad, etc.).
- **Image de catalogues Asset** -utilisez **Asset catalogues Image** à gérer et de regrouper tous les versions d’une ressource requise par une application donnée.
- **Images dans le concepteur iOS** -le concepteur iOS permet de définir des images pour les contrôles.
- **Les images dans le Code** – utilisez le `UIImage` des méthodes de la classe à charger et utiliser des ressources d’image et les assigner aux contrôles d’interface utilisateur en code c#.
- **Icône de l’application** -définir l’icône d’application requis par chaque application iOS. Voici l’icône appuieront par l’utilisateur à partir de l’écran d’accueil pour lancer l’application iOS. En outre, cette icône est utilisée par le centre de jeu, le cas échéant.
- **Mettre en évidence icône** -définir l’icône d’actualités de l’application. Chaque fois que l’utilisateur entre le nom d’une application dans la recherche Spotlight, cette icône s’affiche.
- **Icône des paramètres** -définir l’application **paramètres** icône. Si l’utilisateur entre le **paramètres** application sur son appareil iOS, cette icône s’affichera à la fin de la liste des paramètres de l’application. 
- **Lancer des écrans** -définir l’écran de lancement de l’application. Une fois que l’utilisateur clique sur l’icône d’application et avant la première vue s’affiche, un écran vide s’affiche. Heureusement, iOS prend en charge l’affichage d’une image à la place de l’écran vide à l’aide d’un plan conceptuel. 
- **Icône d’iTunes** -fournir une icône iTune. Si vous utilisez la méthode Ad Hoc de la livraison d’une application (que ce soit pour les utilisateurs d’entreprise ou bêta sur les appareils réels), le développeur doit également inclure un 512 x 512 et une image de 1024 x 1024 qui permet de représenter l’application dans iTunes.
- **Icônes de document** -utiliser une image sous forme d’icône pour n’importe quel type de document spécifique qui prend en charge ou qui crée une application Xamarin.iOS.

Il existe plusieurs considérations qui doivent être prises en compte lors de la création des ressources d’image pour une application iOS, ainsi que plusieurs endroits où ces ressources seront utilisées. Chacun de ces ont une incidence sur la non seulement combien ressources d’image sera nécessaire, mais la création de ces ressources. Les rubriques suivantes décrivent les types de ressources d’images qui seront nécessaires, comment ces ressources sont incluses dans l’offre groupée de l’application et comment les ressources d’image sont consommés pour fournir les fonctionnalités requises :


## <a name="displaying-an-imageiosapp-fundamentalsimages-iconsdisplaying-an-imagemd"></a>[Affichage d'une image](~/ios/app-fundamentals/images-icons/displaying-an-image.md)

Cet article couvre notamment une ressource d’image dans une application Xamarin.iOS et affichage de cette image à l’aide de code c# ou en l’assignant à un contrôle dans le concepteur iOS.

## <a name="application-iconsiosapp-fundamentalsimages-iconsapp-iconsmd"></a>[Icônes d'application](~/ios/app-fundamentals/images-icons/app-icons.md)

Cet article explique comment y compris et la gestion d’un composant de l’image dans une application Xamarin.iOS à utiliser comme icône de l’application.

## <a name="alternate-app-iconsiosapp-fundamentalsimages-iconsalternate-app-iconsmd"></a>[Autres icônes d’applications](~/ios/app-fundamentals/images-icons/alternate-app-icons.md)

Apple a ajouté plusieurs améliorations à iOS 10.3 qui permettent à une application de gérer son icône :

 - `ApplicationIconBadgeNumber` -Obtient ou définit le badge de l’icône de l’application dans le Springboard.
 - `SupportsAlternateIcons` -If `true` l’application a un autre jeu d’icônes.
 - `AlternateIconName` -Retourne le nom de l’icône de remplacement actuellement sélectionné ou `null` si vous utilisez l’icône du premier.
 - `SetAlternameIconName` -Utilisez cette méthode pour basculer l’icône de l’application sur l’icône de remplacement donné.


## <a name="launch-screensiosapp-fundamentalsimages-iconslaunch-screensmd"></a>[Écrans de lancement](~/ios/app-fundamentals/images-icons/launch-screens.md)

Cet article couvre à l’aide d’un type spécial de la table de montage séquentiel pour fournir un écran de lancement universel pour chaque taille de périphérique iOS et la résolution.

## <a name="custom-document-typesiosapp-fundamentalsimages-iconscustom-document-typesmd"></a>[Types de document personnalisé](~/ios/app-fundamentals/images-icons/custom-document-types.md)

Cet article explique comment y compris et la gestion d’un composant de l’image dans une application Xamarin.iOS à utiliser comme une icône de Type de Document personnalisé.



## <a name="related-links"></a>Liens associés

- [Utilisation d’Images (exemple)](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [Icône personnalisée et les instructions de création d’Image](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
