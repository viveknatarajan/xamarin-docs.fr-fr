---
title: Résumé du chapitre 1. Comment convient-elle Xamarin.Forms ?
description: 'Création d’applications mobiles avec Xamarin.Forms : résumé du chapitre 1. Comment convient-elle Xamarin.Forms ?'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F3F864FF-EE70-49D0-90D1-388889037625
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 2897229b0749b1a6ead805d6ad063603a77f8f0d
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240458"
---
# <a name="summary-of-chapter-1-how-does-xamarinforms-fit-in"></a>Résumé du chapitre 1. Comment convient-elle Xamarin.Forms ?

Une des tâches de programmation plus mauvaises consiste à porter un code base d’une plateforme à l’autre, en particulier si cette plateforme implique un autre langage de programmation. Il est tentant lors du portage du code pour refactoriser ainsi, mais si les deux plateformes doivent être conservées en parallèle, puis les différences entre les deux bases de code seront futures maintenance devient plus difficile.

## <a name="cross-platform-mobile-development"></a>Développement mobile multiplateforme

Ce problème est courant lorsque vous ciblez les plateformes mobiles. Actuellement, il existe deux principales plateformes mobiles, la famille Apple de l’iPhone et iPad exécutant le système d’exploitation iOS et le système d’exploitation Android qui s’exécute sur une gamme de téléphones et tablettes. Une autre plateforme significative est plateforme Windows universelle (UWP de Microsoft), ce qui permet à un programme unique cibler Windows 10 et Windows 10 Mobile.

Un fournisseur de logiciels qui souhaite cibler ces trois plateformes doit faire face à des paradigmes de l’interface utilisateur différentes, les trois différents environnements de développement, les trois interfaces de programmation différents, et&mdash;peut-être plus arriver&mdash; trois langages de programmation différents : Objective-C pour l’iPhone et iPad, Java pour Android et c# pour Windows.

## <a name="the-c-and-net-solution"></a>La solution c# et .NET

Bien que Objective-C, Java et c# sont toutes dérivées de langage de programmation C, ils ont évolué en chemins d’accès très différentes. C# est la plus récente de ces langages et a été affinage de manières très utile. En outre, c# est étroitement associé à une infrastructure de programmation entière appelée .NET, qui fournit la prise en charge pour les mathématiques, débogage, réflexion, collections, globalisation, d’e/s de fichier, mise en réseau, sécurité, les threads, les services web, la gestion des données et XML et JSON en lecture et en écriture.

Xamarin fournit actuellement des outils pour cibler le Mac, iOS et Android API à l’aide de c# et .NET native. Ces outils sont appelés Xamarin.Mac, Xamarin.iOS et Xamarin.Android, appelés collectivement la plateforme de Xamarin. Il s’agit des bibliothèques et des liaisons qui expriment les API natives de ces plateformes avec les langages .NET.

Les développeurs peuvent utiliser la plateforme de Xamarin pour écrire des applications ciblant Mac, iOS ou Android en c#. Mais si vous ciblez plusieurs plateformes, il est judicieux de partager du code entre les plateformes cibles. Cela implique la séparation le programme en code dépend de la plateforme (généralement impliquant l’interface utilisateur) et de code indépendant de la plateforme, ce qui nécessite généralement seulement l’infrastructure .NET de base. Ce code indépendant de la plateforme peut résider soit dans une bibliothèque de classes Portable (PCL) ou un projet partagé, souvent appelé projet d’actifs partagés ou SAP.

## <a name="introducing-xamarinforms"></a>Présentation de Xamarin.Forms

Lorsque vous ciblez plusieurs plateformes mobiles, Xamarin.Forms permet de partager le code encore plus. Un seul programme écrit pour Xamarin.Forms peut cibler des plateformes distinctes cinq :

- e/s pour les programmes qui s’exécutent sur l’iPhone, iPad et iPod touch
- Android pour les programmes qui s’exécutent sur les téléphones et tablettes Android
- la plateforme Windows universelle pour cible Windows 10 et Windows 10 Mobile
- API Windows Runtime de Windows 8.1
- API Windows Runtime de Windows Phone 8.1

Les modèles de solution Xamarin.Forms actuelles ne comprennent pas les modèles de projets pour les plateformes Windows 8.1 et Windows Phone 8.1.

Le bloc d’un programme Xamarin.Forms existe dans une bibliothèque de classes portables ou un SAP. Chacune des plateformes se compose d’un stub de petite application qui appelle la bibliothèque PCL. Xamarin.Forms APIs mapper à des contrôles natifs sur chaque plateforme, afin que chaque plate-forme conserve son apparence caractéristique :

[![Capture d’écran de triple des éléments visuels de plateforme partage](images/ch01fg03-small.png "Xamarin.Forms des contrôles sur chaque plateforme")](images/ch01fg03-large.png#lightbox "Xamarin.Forms des contrôles sur chaque plateforme")

Les captures d’écran de gauche à droite montrent un iPhone, un téléphone Android et un téléphone Windows 10 Mobile. Dans chaque écran, la page contient un Xamarin.Forms [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) pour afficher du texte, un [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) pour l’initiation d’actions, un [ `Switch` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) pour choix d’une valeur activé/désactivé et un [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) pour spécifier une valeur dans une plage continue. Les quatre de ces vues sont des enfants d’un [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) sur un [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/).

Également attachée à la page est une barre d’outils Xamarin.Forms composé de plusieurs [ `ToolbarItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/) objets. Ils sont visibles sous forme d’icônes en haut de l’iOS et Android écrans et dans la partie inférieure de l’écran de Windows 10 Mobile.

Xamarin.Forms prend également en charge XAML, la Extensible Application Markup Language développés par Microsoft pour plusieurs plateformes d’application. Tous les éléments visuels du programme ci-dessus sont définis en XAML, comme illustré dans le [ **PlatformVisuals** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01/PlatformVisuals) exemple.

Un programme Xamarin.Forms peut déterminer quelle plate-forme, elle s’exécute sur et exécuter un code différent en conséquence. Plus efficacement, les développeurs peuvent écrire du code personnalisé pour les différentes plates-formes et exécuter ce code à partir d’un programme Xamarin.Forms de façon indépendante de la plate-forme. Les développeurs peuvent également créer des contrôles supplémentaires en écrivant des convertisseurs pour chaque plateforme.

Xamarin.Forms est une bonne solution pour les applications line-of-business, ou pour le prototypage ou effectuer une démonstration de preuve de concept rapide, il est moins idéal pour les applications qui nécessitent des graphismes vectoriels ou une interaction de tactile complexes.

## <a name="your-development-environment"></a>Votre environnement de développement

Votre environnement de développement dépend de ce que vous souhaitez cibler des plateformes et les ordinateurs que vous voulez utiliser.

Si vous souhaitez cible iOS, vous devez un Mac avec Xcode et la plateforme Xamarin installé. Prise en charge d’Android ainsi nécessite l’installation de Java et les kits de développement logiciel requis. Vous pouvez cibler iOS et Android à l’aide de Visual Studio pour Mac.

Installation de Visual Studio vous permet de sur le PC cibler iOS, Android et toutes les plateformes Windows. Toutefois, cibler iOS à partir de Visual Studio requiert toujours un Mac avec Xcode et la plateforme Xamarin installé.

Vous pouvez tester les programmes sur un appareil réel que connectés par USB à l’ordinateur, ou dans un simulateur.

## <a name="installation"></a>Installation

Avant de créer et de création d’une application de Xamarin.Forms, il est conseillé créer et générer séparément une application iOS, une application Android et une application UWP, selon les plateformes que vous souhaitez cible et votre environnement de développement.

Les sites web de Xamarin et Microsoft contiennent des informations sur la procédure à suivre :

- [Prise en main d’e/s](~/ios/get-started/index.md)
- [Mise en route avec Android](~/android/get-started/index.md)
- [Centre de développement Windows](http://dev.windows.com)

Une fois que vous pouvez créer et n’exécuter des projets pour ces plateformes individuels, vous devez avoir aucun problème de création et exécution d’une application de Xamarin.Forms.



## <a name="related-links"></a>Liens associés

- [Recherche en texte intégral chapitre 1 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch01-Apr2016.pdf)
- [Exemple de chapitre 1](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)
