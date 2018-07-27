---
title: Utilisation de projets à partager du Code partagés
description: Projets partagés vous permettent d’écrire du code commun qui est référencé par un nombre de projets d’application différents. Le code est compilé dans le cadre de chaque projet de référence et peut inclure des directives de compilateur pour aider à intégrer les fonctionnalités spécifiques à la plateforme dans la base de code partagé.
ms.prod: xamarin
ms.assetid: 191c71fb-44a4-4e6c-af4b-7b1107dce6af
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 61096635cd94d0fdd0abe6fda59c4efa41eeceb1
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270213"
---
# <a name="shared-projects-code-sharing"></a>Partagé de partage de code de projets

_Projets partagés vous permettent d’écrire du code commun qui est référencé par un nombre de projets d’application différents. Le code est compilé dans le cadre de chaque projet de référence et peut inclure des directives de compilateur pour aider à intégrer les fonctionnalités spécifiques à la plateforme dans la base de code partagé._

Projets partagés (également appelés projets actifs partagés) vous permettent d’écrire du code qui est partagé entre plusieurs projets de cible, y compris les applications Xamarin.

Ils prennent en charge les directives de compilateur afin que vous pouvez inclure de façon conditionnelle code spécifique à la plateforme pour être compilé dans un sous-ensemble de projets qui référencent le projet partagé. Il est également prise en charge IDE pour aider à gérer les directives du compilateur et de visualiser l’aspect du code dans chaque application.

Si vous avez utilisé la liaison de fichiers dans le passé pour partager du code entre les projets, les projets partagés fonctionne de manière similaire, mais avec prise en charge des IDE beaucoup améliorée.

## <a name="what-is-a-shared-project"></a>Qu’est un projet partagé ?

Contrairement à la plupart des autres types de projet, un projet partagé n’a pas de toute sortie (sous forme DLL), au lieu de cela, le code est compilé dans chaque projet qui y fait référence. Ceci est illustré dans le diagramme ci-dessous : sur le plan conceptuel tout le contenu du projet partagé est « copié dans « chaque projet de référence et compilée comme si elle faisait partie d'entre eux.

![](shared-projects-images/sharedassetproject.png "Architecture de projet partagé")

Le code dans un projet partagé peut contenir des directives de compilateur qui active ou désactive les sections de code en fonction de l’application qui projet utilise le code, ce qui est proposé par les zones de couleur de plateforme dans le diagramme.

Un projet partagé ne pas obtenir compilé sur son propre, il existe uniquement comme un regroupement de fichiers de code source qui peut être inclus dans d’autres projets. Quand référencée par un autre projet, le code est effectivement compilé en tant que *partie* de ce projet. Projets partagés ne peut pas faire référence à n’importe quel autre type de projet (y compris les autres projets partagés).

Notez que les projets d’application Android ne peut pas référencer d’autres projets d’application Android, par exemple, un projet de test unitaire Android ne peut pas référencer un projet d’application Android. Pour plus d’informations sur cette limitation, consultez ce [forum de discussion](http://forums.xamarin.com/discussion/comment/98092/).

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

## <a name="visual-studio-for-mac-walkthrough"></a>Procédure pas à pas Visual Studio pour Mac

Cette section décrit comment créer et utiliser un projet partagé à l’aide de Visual Studio pour Mac. Consultez le pour [exemple de projet partagé](#Shared_Project_Example) section pour obtenir un exemple complet.

## <a name="creating-a-shared-project"></a>Création d’un projet partagé

Pour créer un nouveau projet partagé accédez à **fichier > Nouvelle Solution...**  (ou cliquez avec le bouton droit sur une solution existante et en choisissant **Ajouter > Ajouter un nouveau projet...** ):

[![Nouveau projet partagé](shared-projects-images/xs-newsolution-sml.png "nouvelle Solution")](shared-projects-images/xs-newsolution.png#lightbox)

Dans l’écran suivant, choisissez le nom du projet, puis cliquez sur **créer**.

Un projet partagé est montré ci-dessous : Notez qu’il y a aucune référence ou nœuds de composants ; ils ne sont pas pris en charge pour les projets partagés.

![Vide de projet partagé](shared-projects-images/xs-empty.png "vide de projet partagé")

Pour un projet partagé soit utile, il doit être référencé au moins un projet en mesure de build (par exemple, un iOS ou Android application ou bibliothèque ou un projet de bibliothèque de classes portable). Un projet partagé ne pas compilé lorsqu’il n’a rien de référencement, c’est le cas syntaxe (ou tout autre) erreurs ne seront pas mis en surbrillance jusqu'à ce qu’il a été référencé par un autre processus.

Ajout d’une référence à un projet partagé s’effectue de la même façon que la référence d’un projet de bibliothèque standard. Cette capture d’écran montre un projet Xamarin.iOS faisant référence à un projet partagé.

![](shared-projects-images/xs-reference.png "Référence de projet à projet partagé")

Une fois que le projet partagé est référencé par une autre bibliothèque ou une application, vous pouvez générer la solution et afficher toutes les erreurs dans le code. Lorsque le projet partagé est référencé par _deux ou plusieurs_ autres projets, un menu s’affiche dans l’angle supérieur gauche de l’éditeur de code source que montre choisir les projets qui font référence à ce fichier.

## <a name="shared-project-options"></a>Options de projet partagées

Lorsque vous cliquez sur un projet partagé et choisissez **Options** il moins de paramètres que les autres types de projets. Étant donné que les projets partagés ne sont pas compilées (sur leurs propres), Impossible de définir les options du compilateur ou de sortie, les configurations de projet, signature d’assembly ou des commandes personnalisées. Le code dans un projet partagé efficacement hérite de ces valeurs tout ce qui fait référence à eux.



Le **Options** écran est illustré ci-dessous : le projet **nom** et **Namespace par défaut** sont les uniquement deux paramètres que vous modifierez généralement.


![](shared-projects-images/xs-sharedprojectoptions.png "Options de projet partagées")



# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)



## <a name="visual-studio-walkthrough"></a>Procédure pas à pas Visual Studio


Cette section décrit comment créer et utiliser un projet partagé à l’aide de Visual Studio. Consultez le pour [exemple de projet partagé](#Shared_Project_Example) section pour une implémentation complète.

### <a name="creating-a-shared-project"></a>Création d’un projet partagé

Pour créer un nouveau projet partagé accédez à **fichier > Nouvelle Solution...**  et choisissez un nom pour le projet et la solution.

![](shared-projects-images/vs-newsolution.png "Nouvelle solution")

Vous pouvez également ajouter un nouveau projet partagé à une solution en cliquant sur le fichier solution et en choisissant **Ajouter > Nouveau projet...** . Un nouveau projet partagé se présente comme indiqué ci-dessous (après l’ajout d’un fichier de classe) : Notez il n’y a aucune référence ou nœuds de composants ; ils ne sont pas pris en charge pour les projets partagés.

![](shared-projects-images/vs-empty.png "Projet partagé vide")

Pour un projet partagé soit utile, il doit être référencé au moins un projet en mesure de build (par exemple, un iOS ou Android application ou bibliothèque ou un projet de bibliothèque de classes portable). Un projet partagé ne pas compilé lorsqu’il n’a rien de référencement, c’est le cas syntaxe (ou tout autre) erreurs ne seront pas mis en surbrillance jusqu'à ce qu’il a été référencé par un autre processus.

Ajout d’une référence à un projet partagé s’effectue de la même façon que la référence d’un projet de bibliothèque standard. Cette capture d’écran montre un projet Xamarin.iOS faisant référence à un projet partagé.

![](shared-projects-images/vs-reference.png "Référence de projet à projet partagé")

Une fois que le projet partagé est référencé par une autre bibliothèque ou une application, vous pouvez générer la solution et afficher toutes les erreurs dans le code. Lorsque le projet partagé est référencé par _deux ou plusieurs_ autres projets, un menu s’affiche dans l’angle supérieur gauche de l’éditeur de code source pour voir les projets qui référencent le fichier de code actuel.


### <a name="shared-project-properties"></a>Propriétés de projet partagé


Lorsque vous sélectionnez un projet partagé il moins de paramètres dans le volet de propriétés que les autres types de projet. Étant donné que les projets partagés ne sont pas compilées (sur leurs propres), Impossible de définir les options du compilateur ou de sortie, les configurations de projet, signature d’assembly ou des commandes personnalisées. Le code dans un projet partagé efficacement hérite de ces valeurs tout ce qui fait référence à eux.

Le **propriétés** panneau est illustré ci-dessous - le **racine Namespace** est le seul paramètre que vous pouvez modifier.

![](shared-projects-images/vs-sharedprojectproperties.png "Propriétés de projet partagé")

-----

<a name="Shared_Project_Example"/>

## <a name="shared-project-example"></a>Exemple de projet partagé

Le [Tasky](https://github.com/xamarin/mobile-samples/tree/master/Tasky) exemple utilise un projet partagé pour contenir le code commun utilisé par les deux l’iOS, Android et Windows Phone applications. À la fois le `SQLite.cs` et `TaskRepository.cs` les fichiers de code source utilisent des directives du compilateur (par exemple). `#if __ANDROID__`) pour produire un résultat différent pour chacune des applications qui les référencent.

La structure de solution complète est indiquée ci-dessous (dans Visual Studio pour Mac et Visual Studio respectivement) :

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

![](shared-projects-images/xs-examplesolution.png "Visual Studio pour la solution de Mac")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](shared-projects-images/vs-examplesolution.png "Solution Visual Studio")

-----

Le projet Windows Phone sont accessibles à partir de Visual Studio pour Mac, même si ce type de projet n’est pas pris en charge pour la compilation dans Visual Studio pour Mac.

Les applications en cours d’exécution sont présentées ci-dessous :

![](shared-projects-images/example.png "obtenir des exemples iOS, Android, Windows Phone")

## <a name="summary"></a>Récapitulatif

Ce document décrit le fonctionnement des projets partagés, comment ils peuvent être créées et utilisées dans Visual Studio pour Mac et Visual Studio et introduit un exemple simple d’application qui illustre un projet partagé en action.

## <a name="related-links"></a>Liens associés

- [Tasky exemple d’application](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [Bibliothèques de classes portables (exemple)](~/cross-platform/app-fundamentals/pcl.md)
- [Partage de Code, Options (exemple)](~/cross-platform/app-fundamentals/code-sharing.md)
