---
title: Projets partagés
description: Projets partagés vous permettent d’écrire du code commun qui est référencé par un nombre de projets d’application différent. Le code est compilé dans le cadre de chaque projet de référence et peut inclure des directives de compilateur pour aider à intégrer les fonctionnalités spécifiques à la plateforme dans la base de code partagé.
ms.prod: xamarin
ms.assetid: 191c71fb-44a4-4e6c-af4b-7b1107dce6af
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: a7d5a25c3c6f2889bde0ff4aeaf85053bcc796fb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="shared-projects"></a>Projets partagés

_Projets partagés vous permettent d’écrire du code commun qui est référencé par un nombre de projets d’application différent. Le code est compilé dans le cadre de chaque projet de référence et peut inclure des directives de compilateur pour aider à intégrer les fonctionnalités spécifiques à la plateforme dans la base de code partagé._

Les projets partagés (également appelés projets d’actifs partagés) vous permettent d’écrire du code qui est partagé entre plusieurs projets de cible, y compris les applications Xamarin.

Ils prennent en charge les directives du compilateur afin que vous pouvez inclure conditionnelle de code spécifique à la plateforme peut être compilé dans un sous-ensemble des projets qui référencent le projet partagé. Il est également prise en charge IDE pour aider à gérer les directives du compilateur et de visualiser la façon dont le code ressemble dans chaque application.

Si vous avez utilisé la liaison de fichiers dans le passé pour partager du code entre les projets, les projets partagés fonctionne de façon similaire, mais avec prise en charge IDE considérablement améliorée.



## <a name="what-is-a-shared-project"></a>Qu’est un projet partagé ?

Contrairement à la plupart des autres types de projets un projet partagé n’a pas de sortie (sous forme DLL), au lieu de cela, le code est compilé dans chaque projet qui y fait référence. Ceci est illustré dans le diagramme ci-dessous - point de vue conceptuel la totalité du projet partagé est « copiées » chaque projet de référence et compilée comme si elle faisait partie d'entre eux.

 ![](shared-projects-images/sharedassetproject.png "Architecture du projet partagé")

Le code dans un projet partagé peut contenir des directives de compilateur qui active ou désactive les sections de code en fonction de l’application projet utilise le code, ce qui est proposé par les zones de couleur de plateforme dans le diagramme.

Un projet partagé ne pas compilé sur son propre, qu’il existe uniquement comme un regroupement de fichiers de code source qui peut être inclus dans d’autres projets. Quand référencée par un autre projet, le code est effectivement compilé en tant que *partie* de ce projet. Les projets partagés ne peut pas faire référence à n’importe quel autre type de projet (y compris les autres projets partagés).

Notez que les projets d’application Android ne peut pas référencer d’autres projets d’application Android, par exemple, un projet de test unitaire Android ne peut pas faire référence à un projet d’application Android. Pour plus d’informations sur cette limitation, consultez ce [forum de discussion](http://forums.xamarin.com/discussion/comment/98092/).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)



## <a name="visual-studio-for-mac-walkthrough"></a>Procédure pas à pas Visual Studio pour Mac


Cette section vous montre comment créer et utiliser un projet partagé à l’aide de Visual Studio pour Mac. Consultez le pour [exemple de projet partagé](#Shared_Project_Example) section pour obtenir un exemple complet.


## <a name="creating-a-shared-project"></a>Création d’un projet partagé


Pour créer un nouveau projet partagé atteindre **fichier > Nouvelle Solution...**  et choisissez un nom.


![](shared-projects-images/xs-newsolution.png "Nouvelle solution")


Vous pouvez également ajouter un nouveau projet partagé à une solution en cliquant sur le fichier solution et en choisissant **Ajouter > Ajouter un nouveau projet...** . Une nouvelle recherche de projet partagé comme indiqué ci-dessous - Notez il y a aucune référence ou des nœuds de composants ; ils ne sont pas pris en charge pour les projets partagés.


![](shared-projects-images/xs-empty.png "Projet partagé vide")


Pour un projet partagé soit utile, il doit être référencé au moins un projet en mesure de build (par exemple, un iOS ou Android application ou une bibliothèque ou un projet de bibliothèque PCL). Un projet partagé ne pas compilé lorsqu’il n’a rien faisant référence à elle, c’est le cas une syntaxe (ou tout autre) erreurs ne seront pas mis en surbrillance jusqu'à ce qu’il a été référencé par un autre processus.



Ajout d’une référence à un projet partagé est effectuée de la même façon que la référence à un projet de bibliothèque standard. Cette capture d’écran montre un projet Xamarin.iOS faisant référence à un projet partagé.


![](shared-projects-images/xs-reference.png "Référence de projet au projet partagé")


Une fois le projet partagé est référencé par une autre bibliothèque ou une application, vous pouvez générer la solution et afficher toutes les erreurs dans le code. Lorsque le projet partagé est référencé par _deux ou plusieurs_ autres projets, un menu s’affiche dans le coin supérieur gauche de l’éditeur de code source que montre choisir les projets qui font référence à ce fichier.



## <a name="shared-project-options"></a>Options de projet partagées


Lorsque vous cliquez sur un projet partagé et choisissez **Options** il moins de paramètres que les autres types de projets. Étant donné que les projets partagés ne sont pas compilées (sur leurs propres), vous ne peut pas définir les options du compilateur ou de sortie, les configurations de projet, la signature d’assembly ou des commandes personnalisées. Le code dans un projet partagé hérite efficacement ces valeurs à partir de tout ce qui les référence.



Le **Options** écran ci-dessous - le projet **nom** et **par défaut de Namespace** sont généralement modifier les paramètres d’uniquement deux.


![](shared-projects-images/xs-sharedprojectoptions.png "Options de projet partagées")



# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)



## <a name="visual-studio-walkthrough"></a>Procédure pas à pas Visual Studio


Cette section décrit comment créer et utiliser un projet partagé à l’aide de Visual Studio. Consultez le pour [exemple de projet partagé](#Shared_Project_Example) section pour une implémentation complète.


### <a name="creating-a-shared-project"></a>Création d’un projet partagé


Pour créer un nouveau projet partagé atteindre **fichier > Nouvelle Solution...**  et choisissez un nom pour le projet et la solution.


![](shared-projects-images/vs-newsolution.png "Nouvelle solution")


Vous pouvez également ajouter un nouveau projet partagé à une solution en cliquant sur le fichier solution et en choisissant **Ajouter > Nouveau projet...** . Un projet partagé ressemble à ceci (après l’ajout d’un fichier de classe) - Notez il n’y a aucune référence ou des nœuds de composants ; ils ne sont pas pris en charge pour les projets partagés.


![](shared-projects-images/vs-empty.png "Projet partagé vide")


Pour un projet partagé soit utile, il doit être référencé au moins un projet en mesure de build (par exemple, un iOS ou Android application ou une bibliothèque ou un projet de bibliothèque PCL). Un projet partagé ne pas compilé lorsqu’il n’a rien faisant référence à elle, c’est le cas une syntaxe (ou tout autre) erreurs ne seront pas mis en surbrillance jusqu'à ce qu’il a été référencé par un autre processus.



Ajout d’une référence à un projet partagé est effectuée de la même façon que la référence à un projet de bibliothèque standard. Cette capture d’écran montre un projet Xamarin.iOS faisant référence à un projet partagé.


![](shared-projects-images/vs-reference.png "Référence de projet au projet partagé")


Une fois le projet partagé est référencé par une autre bibliothèque ou une application, vous pouvez générer la solution et afficher toutes les erreurs dans le code. Lorsque le projet partagé est référencé par _deux ou plusieurs_ autres projets, un menu s’affiche dans le coin supérieur gauche de l’éditeur de code source pour voir les projets qui référencent le fichier de code en cours.


### <a name="shared-project-properties"></a>Propriétés du projet partagé


Lorsque vous sélectionnez un projet partagé il moins de paramètres dans le volet de propriétés que les autres types de projet. Étant donné que les projets partagés ne sont pas compilées (sur leurs propres), vous ne peut pas définir les options du compilateur ou de sortie, les configurations de projet, la signature d’assembly ou des commandes personnalisées. Le code dans un projet partagé hérite efficacement ces valeurs à partir de tout ce qui les référence.



Le **propriétés** Panneau de configuration est indiqué ci-dessous - le **racine Namespace** est le seul paramètre que vous pouvez modifier.


![](shared-projects-images/vs-sharedprojectproperties.png "Propriétés du projet partagé")



-----

<a name="Shared_Project_Example"/>

## <a name="shared-project-example"></a>Exemple de projet partagé

Le [Tasky](https://github.com/xamarin/mobile-samples/tree/master/Tasky) exemple utilise un projet partagé pour contenir le code commun utilisé par les deux iOS, Android et Windows Phone applications. À la fois le `SQLite.cs` et `TaskRepository.cs` les fichiers de code source utilisent des directives du compilateur (par exemple). `#if __ANDROID__`) pour produire une sortie différente pour chacune des applications qui les référencent.

Voici la structure de la solution complète (dans Visual Studio pour Mac et Visual Studio respectivement) :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

 ![](shared-projects-images/xs-examplesolution.png "Visual Studio pour les solutions Mac")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

 ![](shared-projects-images/vs-examplesolution.png "Solution Visual Studio")

-----

Le projet Windows Phone peut être parcouru à partir de Visual Studio pour Mac, même si ce type de projet n’est pas prise en charge pour la compilation dans Visual Studio pour Mac.

Les applications en cours d’exécution sont indiquées ci-dessous.

 ![](shared-projects-images/example.png "obtenir des exemples iOS, Android, Windows Phone")



## <a name="summary"></a>Récapitulatif

Ce document décrit comment les projets partagés fonctionnent, comment ils peuvent être créés et utilisés dans Visual Studio pour Mac et Visual Studio et a introduit un exemple simple d’application qui illustre un projet partagé dans l’action.

## <a name="related-links"></a>Liens associés

- [Tasky exemple d’application](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [Bibliothèques de classes portables (exemple)](~/cross-platform/app-fundamentals/pcl.md)
- [Partage des Options de Code (exemple)](~/cross-platform/app-fundamentals/code-sharing.md)
