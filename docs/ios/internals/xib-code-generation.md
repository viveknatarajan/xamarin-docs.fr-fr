---
title: "Génération de code .xib"
ms.topic: article
ms.prod: xamarin
ms.assetid: 365991A8-E07A-0420-D28E-BC4D32065E1A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 7d320b3fc40c852c337e5fd1e9bda4e90920cf70
ms.sourcegitcommit: cc38757f56aab53bce200e40f873eb8d0e5393c3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/20/2018
---
# <a name="xib-code-generation"></a>Génération de code .xib

> [!IMPORTANT]
>  Ce document explique Visual Studio pour l’intégration du Mac avec le Générateur de Xcode Interface uniquement, comme les Actions et les sorties ne sont pas utilisées dans le Concepteur de Xamarin pour iOS. Pour plus d’informations sur le concepteur iOS, passez en revue les [iOS concepteur](~/ios/user-interface/designer/index.md) document.

L’outil Apple Interface Générateur (« i ») peut être utilisé pour concevoir visuellement des interfaces utilisateur. Les définitions d’interface créées par IB sont enregistrées dans **.xib** fichiers. Widgets et autres objets dans **.xib** fichiers peuvent être attribués à une « classe identité », qui peut être un type personnalisé défini par l’utilisateur. Cela vous permet de personnaliser le comportement de widgets et d’écrire des widgets personnalisés.

Ces classes sont normalement des sous-classes de classes de contrôleur d’interface utilisateur. Ils ont *prises* (analogues aux propriétés) et *actions* (analogue à des événements) qui peut être connecté aux objets d’interface. Lors de l’exécution, lorsque le fichier IB est chargé, les objets sont créés et les sorties et les actions sont connectées aux divers objets de l’interface utilisateur dynamique. Lorsque vous définissez ces classes managées, vous devez définir toutes les actions et les prises de courant pour correspondre à ceux IB attend. Visual Studio pour Mac utilise un modèle code-behind de type simplifier cet affichage. Ceci est similaire à ce que fait par Xcode pour Objective-C, mais les conventions et le modèle de génération du code ont été modifiées pour être plus facile pour les développeurs .NET.

Utilisation de **.xib** n'est pas actuellement pris en charge dans Xamarin.iOS pour Visual Studio.

## <a name="xib-files-and-custom-classes"></a>les fichiers .xib et Classes personnalisées

Ainsi qu’à l’aide des types existants à partir / Cocoa tactile, il est possible de définir des types personnalisés dans **.xib** fichiers. Il est également possible d’utiliser des types qui sont définis dans d’autres **.xib** fichiers, ou défini en code c#. Actuellement, le constructeur d’Interface n’est pas connaître les détails des types définis en dehors de le **.xib** de fichiers, et elle ne sera pas les répertorier ou afficher leurs prises personnalisés et les actions. Suppression de cette limitation est prévue pour une date ultérieure.

Les classes personnalisées peuvent être définies dans un **.xib** à l’aide de la commande « Ajouter sous-classe » dans l’onglet « Classes » de l’Interface Générateur de fichiers. Nous nous référons à ces classes de « Code-behind ». Si le **.xib** fichier comporte un «. xib.designer.cs » fichier équivalent dans le projet, puis Visual Studio pour Mac sera automatiquement remplir avec des définitions de classes partielles pour toutes les classes personnalisées dans le **.xib**. Nous appelons ces classes partielles « classes de concepteur ».

## <a name="generating-code"></a>Génération du code

Pour toute **{0} .xib** fichier avec une action de génération *Page*, si un **.xib.designer.cs {0}** fichier existe également dans le projet, Visual Studio pour Mac génère des classes partielles dans le fichier de concepteur pour toutes les classes d’utilisateur trouvé à la **.xib** fichier, avec les méthodes partielles pour toutes les actions et les propriétés pour les sorties. Génération de code est activée simplement par la présence de ce fichier.

Le fichier de concepteur est automatiquement mis à jour lorsque le **.xib** fichier pour Mac reprend les modifications et Visual Studio. Le fichier du concepteur ne doit pas être modifié manuellement, comme les modifications seront remplacée prochaine Visual Studio pour les mises à jour Mac le fichier.

## <a name="registration-and-namespaces"></a>L’inscription et les espaces de noms

Visual Studio pour Mac génère les classes de concepteur à l’aide d’espace de noms du projet par défaut pour l’emplacement du fichier de concepteur, pour le rendre cohérent avec namespacing de projet .NET normal. L’espace de noms de fichiers de concepteur est piloté par « espace de noms » du projet et ses paramètres « Stratégies d’appellation .NET ». Veillez à ce que si les modifications de l’espace de noms de votre projet par défaut, MD va être régénérée les classes dans l’espace de noms, donc vous souhaiterez peut-être que vos classes partielles ne correspondent plus.

Pour rendre la classe détectable par le runtime Objective-C, Visual Studio pour Mac s’applique une `[Register (name)]` à la classe d’attributs. Bien que Xamarin.iOS enregistre automatiquement `NSObject`-classes dérivées, il utilise les noms complets de .NET. L’attribut est appliqué par Visual Studio pour Mac substitue cette option pour vous assurer de chaque classe est inscrit avec le nom utilisé dans le **.xib** fichier. Si vous utilisez des classes personnalisées dans IB sans utiliser Visual Studio pour Mac pour générer des fichiers de concepteur, vous devrez peut-être appliquer manuellement pour rendre vos classes managées correspondent aux noms de classes Objective-C attendus.

Les classes ne peuvent pas être définies dans plusieurs **.xib**, ou ils entrent en conflit.

## <a name="non-designer-class-parts"></a>Classe de concepteur non parties

Classes partielles concepteur ne sont pas destinées à être utilisée en tant que-est. Prises sont privées, et aucune classe de base n’est spécifié. Il est attendu que chaque classe possède une partie « non concepteur » classe correspondante dans un autre fichier, qui définit la classe de base, qui utilise ou expose les prises et définit des constructeurs qui sont requis pour instancier la classe à partir du code natif lors du chargement de la **.xib**. La valeur par défaut **.xib** modèles pour ce faire, mais pour les classes personnalisées supplémentaires que vous définissez dans un **.xib**, vous devez ajouter manuellement la partie non-concepteur.

La raison à cela est nécessaire pour une grande souplesse. Par exemple, plusieurs classes de code-behind a pu sous-classe que commune gérés classe abstraite, les sous-classes de la classe pour être sous-classé par IB.

Il est classique placer un **.xib.cs {0}** en regard du fichier le **{0}.xib.designer.cs** fichier de concepteur.

<a name="generated" />

## <a name="generated-actions-and-outlets"></a>Prises et Actions générées

Dans les classes de concepteur partielles, Visual Studio pour Mac génère des propriétés correspondant à n’importe quel prises connectés définis dans IB et méthodes partielles correspondant à toutes les actions connectées.

### <a name="outlet-properties"></a>Propriétés de la sortie

Classes de concepteur contiennent des propriétés correspondant à toutes les sorties définies sur la classe personnalisée. Le fait qu’il s’agit de propriétés est un détail d’implémentation de la Xamarin.iOS pour Objective C pont, pour activer la liaison tardive. Vous devez envisager de les rendre équivalents aux champs privés, destinées à être utilisée uniquement à partir de la classe code-behind. Si vous souhaitez les rendre publiques, ajouter des propriétés d’accesseur pour la partie de la classe de concepteur non, comme vous le feriez pour tout autre champ privé.

Si les propriétés de la sortie sont définies pour avoir un type de `id` (équivalent à `NSObject`), puis le Générateur de code concepteur détermine actuellement le type de possible les plus forts basé sur des objets connectés à ce magasin, pour des raisons pratiques.
Toutefois, cela ne peut pas être pris en charge dans les versions ultérieures, il est donc recommandé que vous tapez explicitement fortement les prises lors de la définition de la classe personnalisée.

### <a name="action-properties"></a>Propriétés d’une action

Classes de concepteur contiennent des méthodes partielles correspondant à toutes les actions définies dans la classe personnalisée. Il s’agit des méthodes sans implémentation. L’objectif des méthodes partielles est double :

1.  Si vous tapez `partial` dans le corps de la classe de la partie de la classe de concepteur non, Visual Studio pour Mac proposera de saisie semi-automatique les signatures de toutes les méthodes partielles non implémentée.
2.  Les signatures de méthode partielle ont un attribut appliqué, ce qui les expose au monde Objective-C, afin qu’ils obtenir traités en tant que l’action correspondante.


Si vous le souhaitez, vous pouvez ignorer la méthode partielle et l’implémentation de l’action en appliquant l’attribut à une méthode différente ou laisser passer à une classe de base.

Si les actions sont définies pour avoir un type d’expéditeur de `id` (équivalent à `NSObject`), le Générateur de code concepteur actuellement détermine que le type possible les plus forts basé sur des objets liés à cette action. Toutefois, cela ne peut pas être pris en charge dans les versions ultérieures, il est donc recommandé que vous tapez explicitement fortement les actions lors de la définition de la classe personnalisée.

Notez que ces méthodes partielles sont créés uniquement pour c#, car CodeDOM ne prend en charge les méthodes partielles, afin qu’ils ne sont pas générés pour les autres langages.

## <a name="cross-xib-class-usage"></a>Utilisation des classes de cross-XIB

Parfois, les utilisateurs souhaitent faire référence à la même classe à partir de plusieurs **.xib** fichiers, par exemple avec des contrôleurs de l’onglet. Cela est possible en explicitement faisant référence à la définition de classe à partir d’un autre **.xib** de fichiers ou en les définissant le même nom de classe dans la seconde **.xib**.

Le dernier cas peut être problématique, car Visual Studio pour le traitement de Mac **.xib** fichiers individuellement. Il ne peut pas détecter automatiquement et fusionner des définitions en double, donc vous risquez conflits appliquant l’attribut d’inscrire plusieurs fois lors de la même classe partielle est définie dans plusieurs fichiers de concepteur. Les versions récentes de Visual Studio pour Mac tentent de résoudre ce problème, mais il peut ne pas toujours fonctionner comme prévu. Dans les futures, c’est susceptible de devenir non pris en charge et à la place, Visual Studio pour Mac fera tous les types définis dans toutes les **.xib** fichiers et du code managé dans le projet directement visible de tous les **.xib** fichiers.

## <a name="type-resolution"></a>Résolution de type

Types utilisés dans IB sont des noms de type Objective-C. Elles sont mappées aux types CLR via l’utilisation d’attributs du Registre. Lors de la génération de code de sortie et l’action, Visual Studio pour Mac résoudre les types CLR correspondants pour tous les types de Objective-C encapsulés par le noyau de Xamarin.iOS et leurs noms de type qualifiés complets.

Toutefois, le Générateur de code ne peut pas résoudre actuellement les types CLR à partir des noms de type Objective-C dans le code de l’utilisateur ou de bibliothèques, donc dans ce cas, il affiche le nom de type textuel. Cela signifie que le type CLR correspondant doit avoir le même nom que le type Objective-C et se trouver dans le même espace de noms que le code qui l’utilise. Cela est planifié à résoudre un certain temps à l’avenir en tenant compte de tous les types de Objective-C dans le projet lors de la génération de code.
