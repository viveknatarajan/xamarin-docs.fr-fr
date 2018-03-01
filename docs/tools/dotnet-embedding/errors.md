---
title: "Erreurs de l’incorporation de .NET"
ms.topic: article
ms.prod: xamarin
ms.assetid: 932C3F0C-D968-42D1-BB14-D97C73361983
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 90d30b92069bcd6a5c008fa8009c0392c4d26473
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="em0xxx-binding-error-messages"></a>EM0xxx : messages d’erreur de liaison

E.g. paramètres, environnement

<!-- 0xxx: the generator itself, e.g. parameters, environment -->
<h3><a name="EM0000"/>EM0000 : Une erreur inattendue - Veuillez remplir un rapport de bogue à https://github.com/mono/Embeddinator-4000/issues</h3>

Une condition d’erreur inattendue s’est produite. Veuillez [un problème](https://github.com/mono/Embeddinator-4000/issues) avec autant d’informations que possible, notamment :

* Journaux, de la génération complète avec un maximum de commentaires
* Un cas de test minimale que reproduire l’erreur
* Toutes les informations de version

Pour obtenir des informations sur la version exacte le plus simple consiste à utiliser le **Xamarin Studio** menu, **sur Xamarin Studio** élément, **afficher les détails** bouton et copier/coller la version informations (vous pouvez utiliser la **copier les informations** bouton).

<h3><a name="EM0001"/>EM0001 : Impossible de créer le répertoire de sortie `X`</h3>

Le nom du répertoire spécifié par `-o=DIR` n’existe pas et ne peut pas être créé. Il peut être un nom non valide pour le système de fichiers.

<h3><a name="EM0002"/>EM0002 : Option `X` n’est pas pris en charge</h3>

L’outil ne prend pas en charge l’option `X`. Il est possible qu’une autre version de l’outil prend en charge ou il ne s’applique pas dans cet environnement.

<h3><a name="EM0003"/>EM0003 : La plateforme `X` n’est pas valide.</h3>

L’outil ne prend pas en charge la plateforme `X`. Il est possible qu’une autre version de l’outil prend en charge ou il ne s’applique pas dans cet environnement.

<h3><a name="EM0004"/>EM0004 : La cible `X` n’est pas valide.</h3>

L’outil ne prend pas en charge la cible `X`. Il est possible qu’une autre version de l’outil prend en charge ou il ne s’applique pas dans cet environnement.

<h3><a name="EM0005"/>EM0005 : La cible de compilation `X` n’est pas valide.</h3>

L’outil ne prend pas en charge la cible de compilation `X`. Il est possible qu’une autre version de l’outil prend en charge ou il ne s’applique pas dans cet environnement.

<h3><a name="EM0006"/>EM0006 : N’a pas pu trouver l’emplacement de Xcode.</h3>

L’outil n’a pas trouvé l’emplacement Xcode actuellement sélectionné en utilisant la `xcode-select -p` commande. Veuillez vérifier que cette commande réussit et retourne l’emplacement correct de Xcode.

<h3><a name="EM0007"/>EM0007 : Impossible d’obtenir la version du Kit de développement logiciel pour '{sdk}'.</h3>

L’outil n’a pas pu obtenir la version du Kit de développement logiciel à l’aide du `xcrun --show-sdk-version --sdk {sdk}` commande. Veuillez vérifier que cette commande réussit et retourne la version du Kit de développement logiciel.

<h3><a name="EM0008"/>EM0008 : L’architecture '{arch}' n’est pas valide pour la plateforme de {}. Les architectures valides pour la plateforme de {} sont : '{architectures}'.</h3>

L’architecture du message d’erreur n’est pas valide pour la plateforme ciblée. Vérifiez que l’option--abi est passée à une architecture valide.

<h3><a name="EM0009"/>EM0009 : La fonctionnalité `X` n’est pas implémentée actuellement par le Générateur</h3>

Il s’agit d’un problème connu que nous avons l’intention de corriger dans une prochaine version du générateur. Contributions sont les bienvenus.

<h3><a name="EM0010"/>EM0010 : Impossible de fusionner les infrastructures '{simulatorFramework}' et '{deviceFramework}', car le fichier ' {}' existe dans les deux.</h3>

L’outil Impossible de fusionner les infrastructures mentionnés dans le message d’erreur, car il existe un fichier commun entre eux.

Cela peut indiquer un bogue dans le Embeddinator-4000 ; Veuillez archiver un rapport de bogue à [https://github.com/mono/Embeddinator-4000/issues](https://github.com/mono/Embeddinator-4000/issues) avec un cas de test.

<h3><a name="EM0011"/>EM0011 : L’assembly `X` n’existe pas.</h3>

L’outil n’a pas pu trouver l’assembly `X` spécifié dans les arguments.

<h3><a name="EM0012"/>EM0012 : Le nom de l’assembly `X` n’est pas unique</h3>

Plusieurs assemblys fourni ont le même nom interne et il ne serait pas possible de les distinguer lors de l’exécution.

La cause la plus probable est qu’un assembly est spécifié plusieurs fois sur les arguments de ligne de commande. Toutefois un conserve toujours renommé assembly il est le nom d’origine et plusieurs copies ne peut pas coexiste.

<h3><a name="EM0013"/>EM0013 : Impossible de trouver l’assembly 'X', référencé par un « Y ».</h3>

L’assembly 'X' référencé par l’assembly 'Y', l’outil est introuvable. Assurez-vous que tous les assemblys référencés sont dans le même répertoire que l’assembly à lier.

<h3><a name="EM0014"/>EM0014 : N’a pas trouvé {product} ({product} {min_version} est requis).</h3>

La dépendance mentionnée dans le message d’erreur est introuvable sur le système.

<h3><a name="EM0015"/>EM0015 : N’a pas pu trouver une version valide de {product} ({version}, trouvée mais au moins {min_version} est requis).</h3>

La dépendance mentionné dans l’erreur du message a été trouvé sur le système, mais il est trop ancien. Mettez à jour vers une version plus récente.

<h3><a name="EM0016">EM0016 : N’a pas pu créer le lien symbolique '{file}' -> '{target}' : erreur {number}</h3>

N’a pas pu créer le lien symbolique mentionné dans le message d’erreur.

<h3><a name="EM0026"/>EM0026 peut pas analyser l’argument de ligne de commande 'A' : *</h3>

La syntaxe pour l’option de ligne de commande `A` n’a pas pu être analysée par l’outil. Il est probable incorrect, veuillez consulter la documentation ou l’aide de la syntaxe correcte.

<h3><a name="EM0099"/>EM0099 : Erreur interne *. Veuillez soumettre un rapport de bogue à un cas de test (https://github.com/mono/Embeddinator-4000/issues).</h3>

Ce message d’erreur est signalé en cas d’échec d’une vérification de cohérence interne dans le Embeddinator-4000.

Cela indique un bogue dans le Embeddinator-4000 ; Veuillez archiver un rapport de bogue à [https://github.com/mono/Embeddinator-4000/issues](https://github.com/mono/Embeddinator-4000/issues) avec un cas de test.


<!-- 1xxx: code processing -->

# <a name="em1xxx-code-processing"></a>EM1xxx : Le traitement du Code

<h3><a name="EM1010"/>Type `T` n’est pas généré, car `X` ne sont pas pris en charge.</h3>

Il s’agit d’un **avertissement** qui le type `T` seront ignorées (autrement dit, rien ne sera générée), car elle utilise `X`, une fonctionnalité qui n’est pas pris en charge.

Remarque : Les fonctionnalités prises en charge évoluera avec les nouvelles versions de l’outil.


<h3><a name="EM1011"/>Type `T` n’est pas généré, car il lui manque un équivalent natif.</h3>

Il s’agit d’un **avertissement** qui le type `T` seront ignorées (autrement dit, rien ne sera générée), car il l’utilise exposent quelque chose à partir de .NET framework qui n’a aucun équivalent dans la plateforme native.


<h3><a name="EM1011"/>Type `T` n’est pas généré, car il ne dispose pas de code de marshaling avec un homologue natif.</h3>

Il s’agit d’un **avertissement** qui le type `T` seront ignorées (autrement dit, rien ne sera générée), car il l’utilise exposent quelque chose à partir de .NET framework qui requiert le marshaling supplémentaires.

Remarque : Il s’agit d’un élément peut obtenir pris en charge, avec certaines limitations, dans une future version de l’outil.


<h3><a name="EM1020"/>Constructeur `C` n’est pas généré en raison du type de paramètre `T` n’est pas pris en charge.</h3>

Il s’agit d’un **avertissement** qui le constructeur `C` seront ignorées (autrement dit, rien ne sera générée), car un paramètre de type `T` n’est pas pris en charge.

Il doit y avoir un avertissement antérieur donner plus d’informations pourquoi tapez `T` n’est pas pris en charge.

Remarque : Les fonctionnalités prises en charge évoluera avec les nouvelles versions de l’outil.


<h3><a name="EM1021"/>Constructeur `C` a des valeurs par défaut pour laquelle aucun wrapper n’est généré.</h3>

Il s’agit d’un **avertissement** que les paramètres par défaut du constructeur `C` n’établissent pas de code supplémentaire. La cause la plus courante est qu’une méthode existante a déjà la même signature. E.g. dans .net, il est possible d’avoir :

```
public class MyType {
    public MyType () { ... }
    public MyType (int i = 0) { ... }
}
```

Dans ce cas que deux généré `init` sélecteurs seront créées, un appel à la fois dans mono, mais aucun wrapper pour les versions ultérieures n’est conservées.


<h3><a name="EM1030"/>Méthode `M` n’est pas généré comme type de retour `T` n’est pas pris en charge.</h3>

Il s’agit d’un **avertissement** qui la méthode `M` seront ignorées (autrement dit, rien ne sera générée), car il est de type de retour `T` n’est pas pris en charge.

Il doit y avoir un avertissement antérieur donner plus d’informations pourquoi tapez `T` n’est pas pris en charge.

Remarque : Les fonctionnalités prises en charge évoluera avec les nouvelles versions de l’outil.


<h3><a name="EM1031"/>Méthode `M` n’est pas généré en raison du type de paramètre `T` n’est pas pris en charge.</h3>

Il s’agit d’un **avertissement** que la méthode `M` sera ignoré (autrement dit, rien ne sera générée), car un paramètre de type `T` n’est pas prise en charge.

Il doit y avoir un avertissement antérieur donner plus d’informations pourquoi tapez `T` n’est pas pris en charge.

Remarque : Les fonctionnalités prises en charge évoluera avec les nouvelles versions de l’outil.


<h3><a name="EM1032"/>Méthode `M` a des valeurs par défaut pour laquelle aucun wrapper n’est généré.</h3>

Il s’agit d’un **avertissement** que les paramètres par défaut de la méthode `M` n’établissent pas de code supplémentaire. La cause la plus courante est qu’une méthode existante a déjà la même signature. E.g. dans .net, il est possible d’avoir :

```
public class MyType {
    public int Increment () { ... }
    public int Increment (int i = 0) { ... }
}
```

Dans ce cas que deux généré `increment` sélecteurs seront créées, un appel à la fois dans mono, mais aucun wrapper pour les versions ultérieures n’est conservées.


<h3><a name="EM1033"/>Méthode `M` n’est pas généré, car une autre méthode expose l’opérateur avec un nom convivial.</h3>

Il s’agit d’un **avertissement** que la méthode `M` n’est pas généré, car une autre méthode expose l’opérateur avec un nom convivial. (https://msdn.microsoft.com/en-us/library/ms229032(v=vs.110).aspx)


<h3><a name="EM1034"/>Méthode d’extension `M` n’est pas généré à l’intérieur d’une catégorie, car ils ne peuvent pas être créés sur le type primitif `T`. Une méthode statique normale a été générée.</h3>

Il s’agit d’un **avertissement** une méthode d’extension sur un primivite de type (par exemple, `System.Int32`) a été trouvé. Dans ObjC, il n’est pas possible de créer des catégories de type primitif. À la place le générateur être produira une méthode statique normale.



<h3><a name="EM1040"/>Propriété `P` n’est pas généré en raison du type de paramètre `T` n’est pas pris en charge.</h3>

Il s’agit d’un **avertissement** qui la propriété `P` seront ignorées (autrement dit, rien ne sera générée), car le type exposé `T` n’est pas pris en charge.

Il doit y avoir un avertissement antérieur donner plus d’informations pourquoi tapez `T` n’est pas pris en charge.

Remarque : Les fonctionnalités prises en charge évoluera avec les nouvelles versions de l’outil.

<h3><a name="EM1041"/>Propriétés indexées sur `T` n’est pas généré, car plusieurs propriétés indexées ne sont pas pris en charge.</h3>

Il s’agit d’un **avertissement** qui les propriétés indexées sur `T` seront ignorées (autrement dit, rien ne sera générée), car plusieurs propriétés indexées ne sont pas pris en charge.


<h3><a name="EM1050"/>Champ `F` n’est pas généré en raison du type de champ `T` n’est pas pris en charge.</h3>

Il s’agit d’un **avertissement** qui le champ `F` seront ignorées (autrement dit, rien ne sera générée), car le type exposé `T` n’est pas pris en charge.

Il doit y avoir un avertissement antérieur donner plus d’informations pourquoi tapez `T` n’est pas pris en charge.

Remarque : Les fonctionnalités prises en charge évoluera avec les nouvelles versions de l’outil.

<h3><a name="EM1051"/>Élément `E` est généré à la place comme `F` , car son nom est en conflit avec un sélecteur d’objectif-c important.</h3>

Il s’agit d’un **avertissement** que l’élément `E` sera généré à la place en tant que `F` , car son nom est en conflit avec un sélecteur d’objectif-c important.

Sélecteurs sur le [NSObjectProtocol](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc) signification important dans objective-c et doit être substituée avec soin.

Remarque : La liste des sélecteurs réservés évoluera avec les nouvelles versions de l’outil.


<!-- 2xxx: code generation -->

# <a name="em2xxx-code-generation"></a>EM2xxx : Génération de Code


<!-- 3xxx: reserved -->
<!-- 4xxx: reserved -->
<!-- 5xxx: reserved -->
<!-- 6xxx: reserved -->
<!-- 7xxx: reserved -->
<!-- 8xxx: reserved -->
<!-- 9xxx: reserved -->
