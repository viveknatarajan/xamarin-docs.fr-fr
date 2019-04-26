---
title: Génération de code .xib dans Xamarin.iOS
description: Ce document décrit comment Xamarin.iOS génère du code pour mapper les fichiers .xib vers C#, rendre des contrôles visuels accessibles par programmation.
ms.prod: xamarin
ms.assetid: 365991A8-E07A-0420-D28E-BC4D32065E1A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 57988374b4383f5659e29edff3834958b8f99f1b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61035910"
---
# <a name="xib-code-generation-in-xamarinios"></a>Génération de code .xib dans Xamarin.iOS

> [!IMPORTANT]
>  Ce document explique de Visual Studio pour l’intégration du Mac avec Interface Builder de Xcode uniquement, comme les Actions et Outlets ne sont pas utilisés dans le Concepteur de Xamarin pour iOS. Pour plus d’informations sur le concepteur iOS, veuillez consulter la [concepteur iOS](~/ios/user-interface/designer/index.md) document.

L’outil d’Interface Builder de Apple i (« b ») peut être utilisé pour concevoir visuellement des interfaces utilisateur. Les définitions d’interface créées par IB sont enregistrées dans **.xib** fichiers. Widgets et autres objets dans **.xib** fichiers peuvent être attribués à une « classe identité », qui peut être un type personnalisé défini par l’utilisateur. Cela vous permet de personnaliser le comportement de widgets et d’écrire des widgets personnalisés.

Ces classes d’utilisateur sont normalement des sous-classes de classes de contrôleur d’interface utilisateur. Ils ont *outlets* (analogues aux propriétés) et *actions* (analogue aux événements) qui peuvent être connectés à des objets d’interface. Lors de l’exécution, lorsque le fichier IB est chargé, les objets sont créés, et les prises de courant et les actions sont connectées aux différents objets interface utilisateur dynamiquement. Lorsque vous définissez ces classes managées, vous devez définir toutes les actions et outlets pour correspondre à ceux qui attend IB. Visual Studio pour Mac utilise un modèle semblable à code-behind pour simplifier cela. Cela est similaire à ce que fait Xcode pour Objective-C, mais les conventions et le modèle de génération de code ont été modifiées pour être plus familières aux développeurs de .NET.

Utilisation de **.xib** fichiers n'est pas actuellement pris en charge dans Xamarin.iOS pour Visual Studio.

## <a name="xib-files-and-custom-classes"></a>fichiers .xib et Classes personnalisées

Ainsi qu’à l’aide des types existants à partir de Cocoa Touch, il est possible de définir des types personnalisés dans **.xib** fichiers. Il est également possible d’utiliser des types qui sont définis dans d’autres **.xib** fichiers, ou définie purement dans C# code. Actuellement, Interface Builder ne connaît pas les détails des types définis en dehors de le **.xib** de fichiers, donc il ne sera pas les répertorier ou afficher leurs prises personnalisés et les actions. Suppression de cette limitation est prévue pour un certain temps à l’avenir.

Classes personnalisées peuvent être définies dans un **.xib** fichier à l’aide de la commande « Ajouter la sous-classe » dans l’onglet « Classes » de l’Interface Builder. Nous faisons référence à ces en tant que classes de « Code-behind ». Si le **.xib** fichier a un «. xib.designer.cs « fichier équivalent dans le projet, puis Visual Studio pour Mac sera automatiquement remplir avec des définitions de classes partielles pour toutes les classes personnalisées dans le **.xib**. Nous faisons référence à ces classes partielles en tant que « classes de concepteur ».

## <a name="generating-code"></a>Génération du code

Pour toute  **{0}.xib** fichier avec une action de génération *Page*, si un  **{0}. xib.designer.cs** fichier existe également dans le projet, Visual Studio pour Mac générer des classes partielles dans le fichier concepteur pour toutes les classes d’utilisateur, il peut trouver dans le **.xib** fichier, avec des propriétés pour les prises de courant et méthodes partielles pour toutes les actions. Génération de code est activée simplement par la présence de ce fichier.

Le fichier de concepteur est automatiquement mis à jour lorsque le **.xib** changements de fichier et Visual Studio pour Mac a regagné le focus. Le fichier de concepteur ne doit pas être modifié manuellement, comme les modifications seront remplacée prochaine Visual Studio pour les mises à jour Mac le fichier.

## <a name="registration-and-namespaces"></a>L’inscription et les espaces de noms

Visual Studio pour Mac génère les classes de concepteur à l’aide d’espace de noms du projet par défaut pour l’emplacement du fichier de concepteur, pour le rendre cohérent avec namespacing de projet .NET normal. L’espace de noms de fichiers de concepteur est déterminé par « par défaut namespace » du projet et ses paramètres « Stratégies d’appellation .NET ». Prenez garde que si l’espace de noms par défaut de votre projet change, MD sera régénérer les classes dans le nouvel espace de noms est donc possible que vos classes partielles ne correspondent pas à.

Pour rendre la classe détectable par le runtime Objective-C, Visual Studio pour Mac s’applique une `[Register (name)]` à la classe d’attributs. Bien que Xamarin.iOS enregistre automatiquement `NSObject`-les classes dérivées, elle utilise les noms .NET qualifié complet. L’attribut appliqué par Visual Studio pour Mac remplace ainsi pour vous assurer de chaque classe est inscrit avec le nom utilisé dans le **.xib** fichier. Si vous utilisez des classes personnalisées dans IB sans utiliser Visual Studio pour Mac pour générer des fichiers de concepteur, vous devrez peut-être appliquer ceci manuellement pour rendre vos classes gérées correspondent aux noms de classe Objective-C attendus.

Les classes ne peuvent pas être définies dans plusieurs **.xib**, ou ils entrent en conflit.

## <a name="non-designer-class-parts"></a>Parties de la classe de concepteur non

Concepteurs classes partielles ne sont pas destinées à être utilisé en tant que-est. Outlets sont privées, et aucune classe de base n’est spécifié. Il est probable que chaque classe aura une partie de « non concepteur » classe correspondante dans un autre fichier, qui définit la classe de base, qui utilise ou expose les prises et définit des constructeurs qui sont requis pour instancier la classe à partir du code natif lors du chargement de la **.xib**. La valeur par défaut **.xib** modèles pour ce faire, mais pour les classes personnalisées supplémentaires que vous définissez dans un **.xib**, vous devez ajouter manuellement la partie non concepteur.

La raison à cela est le besoin de flexibilité. Par exemple, plusieurs classes de code-behind peut sous-classe qu'un commun géré classe abstraite, que les sous-classes de la classe pour être sous-classé par IB.

Il est classique placer ces éléments un  **{0}. xib.cs** en regard du fichier le  **{0}. xib.designer.cs** fichier de concepteur.

<a name="generated" />

## <a name="generated-actions-and-outlets"></a>Outlets et Actions générées

Dans les classes de concepteur partielles, Visual Studio pour Mac génère des propriétés correspondant à n’importe quel prises connectés définis dans IB et méthodes partielles correspondant à toutes les actions connectées.

### <a name="outlet-properties"></a>Propriétés de la sortie

Classes de concepteur contiennent des propriétés correspondant à toutes les prises définis sur la classe personnalisée. Le fait qu’il s’agit de propriétés est un détail d’implémentation de la Xamarin.iOS vers pont Objective C, pour activer la liaison tardive. Vous devez envisager les équivalents aux champs privés, destinées à être utilisée uniquement à partir de la classe code-behind. Si vous souhaitez les rendre publiques, ajouter des propriétés d’accesseur à la partie non-Concepteur de classes, comme vous le feriez pour tout autre champ privé.

Si les propriétés de la sortie sont définies pour avoir un type de `id` (équivalent à `NSObject`) puis le Générateur de code du concepteur actuellement détermine que le type possible plus fort basé sur les objets connectés à cette sortie, pour des raisons pratiques.
Toutefois, cela ne peut pas être pris en charge dans les versions ultérieures, il est donc recommandé que vous tapez explicitement fortement les prises lors de la définition de la classe personnalisée.

### <a name="action-properties"></a>Propriétés d’une action

Classes de concepteur contiennent des méthodes partielles correspondant à toutes les actions définies sur la classe personnalisée. Il s’agit de méthodes sans implémentation. L’objectif des méthodes partielles est double :

1.  Si vous tapez `partial` dans le corps de la classe de la partie de la classe de Concepteur de non, Visual Studio pour Mac offre à la saisie semi-automatique les signatures de toutes les méthodes partielles non implémentée.
2.  Les signatures de méthode partielle ont un attribut appliqué, ce qui les expose au monde Objective-C, ils peuvent obtenir traité en tant que l’action correspondante.


Si vous le souhaitez, vous pouvez ignorer la méthode partielle et implémenter l’action en appliquant l’attribut à une autre méthode ou laissez-le passer à une classe de base.

Si les actions sont définies pour avoir un type d’expéditeur de `id` (équivalent à `NSObject`), puis le Générateur de code du concepteur actuellement détermine que le type possible plus fort basé sur les objets connectés à cette action. Toutefois, cela ne peut pas être pris en charge dans les versions ultérieures, il est donc recommandé que vous tapez explicitement fortement les actions lors de la définition de la classe personnalisée.

Notez que ces méthodes partielles sont créés uniquement pour C#, car le CodeDOM ne prend en charge les méthodes partielles, ils ne sont pas générés pour les autres langues.

## <a name="cross-xib-class-usage"></a>Utilisation des classes de cross-XIB

Parfois, les utilisateurs souhaitent faire référence à la même classe à partir de plusieurs **.xib** fichiers, par exemple avec des contrôleurs de l’onglet. Cela est possible en explicitement faisant référence à la définition de classe à partir d’un autre **.xib** de fichiers, ou en les définissant le même nom de classe dans la seconde **.xib**.

Le dernier cas peut être problématique, car Visual Studio pour le traitement de Mac **.xib** fichiers individuellement. Il ne peut pas automatiquement détecter et fusionner des définitions en double, donc vous risquez présentant des conflits appliquant l’attribut d’inscrire plusieurs fois lors de la même classe partielle est définie dans plusieurs fichiers de concepteur. Les versions récentes de Visual Studio pour Mac tentent de résoudre ce problème, mais il peut ne pas toujours fonctionner comme prévu. Dans un avenir cela est susceptible de devenir non pris en charge et au lieu de cela, Visual Studio pour Mac rendra tous les types définis dans toutes les **.xib** fichiers et le code managé dans le projet directement visible de tous les **.xib** fichiers.

## <a name="type-resolution"></a>Résolution de type

Types utilisés dans IB sont des noms de type Objective-C. Elles sont mappées aux types CLR via l’utilisation d’attributs du Registre. Lors de la génération de code de sortie et l’action, Visual Studio pour Mac résoudre les types CLR correspondants pour tous les types d’Objective-C encapsulés par le noyau de Xamarin.iOS et leurs noms de type qualifiés complets.

Toutefois, le Générateur de code ne peut pas résoudre actuellement les types CLR à partir des noms de type Objective-C dans le code de l’utilisateur ou de bibliothèques, donc dans ce cas, il affiche le nom de type mot pour mot. Cela signifie que le type CLR correspondant doit avoir le même nom que le type Objective-C et se trouver dans le même espace de noms que le code qui l’utilise. Cette intégration est prévue pour être résolu un certain temps à l’avenir en tenant compte de tous les types de Objective-C dans le projet pendant la génération du code.
