---
title: Erreurs d’incorporation .NET
description: Ce document décrit les erreurs générées par l’incorporation de .NET. Les erreurs sont répertoriés par le code et reçoit une description pour aider à résoudre.
ms.prod: xamarin
ms.assetid: 932C3F0C-D968-42D1-BB14-D97C73361983
author: lobrien
ms.author: laobri
ms.date: 04/11/2018
ms.openlocfilehash: 5c3dd406f1132f51a86ddf574ab7ad0b279bc9ec
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61215339"
---
# <a name="net-embedding-errors"></a>Erreurs d’incorporation .NET

## <a name="em0xxx-binding-error-messages"></a>EM0xxx : Messages d’erreur de liaison

Par exemple, paramètres, environnement

<!-- 0xxx: the generator itself, e.g. parameters, environment -->

<a name="EM0000" />

### <a name="em0000-unexpected-error---please-fill-a-bug-report-at-httpsgithubcommonoembeddinator-4000issues"></a>EM0000 : Erreur inattendue : Veuillez remplir un rapport de bogue à https://github.com/mono/Embeddinator-4000/issues

Une condition d’erreur inattendue s’est produite. Veuillez [signaler un problème](https://github.com/mono/Embeddinator-4000/issues) avec autant d’informations que possible, notamment :

* Build complète des journaux, avec un maximum de commentaires
* Un cas de test minimale pour reproduire l’erreur
* Toutes les informations de version

Le moyen le plus simple pour obtenir des informations de version exacte consiste à utiliser le **Xamarin Studio** menu, **sur Xamarin Studio** élément, **afficher les détails** bouton et copier/coller la version informations (vous pouvez utiliser la **copier les informations** bouton).

<a name="EM0001" />

### <a name="em0001-could-not-create-output-directory-x"></a>EM0001 : Impossible de créer répertoire de sortie `X`

Le nom du répertoire spécifié par `-o=DIR` n’existe pas et ne peut pas être créé. Il peut être un nom non valide pour le système de fichiers.

<a name="EM0002" />

### <a name="em0002-option-x-is-not-supported"></a>EM0002 : Option `X` n’est pas pris en charge

L’outil ne prend pas en charge l’option `X`. Il est possible qu’une autre version de l’outil prend en charge ou qu’il ne s’applique pas dans cet environnement.

<a name="EM0003" />

### <a name="em0003-the-platform-x-is-not-valid"></a>EM0003 : La plateforme `X` n’est pas valide.

L’outil ne prend pas en charge la plateforme `X`. Il est possible qu’une autre version de l’outil prend en charge ou qu’il ne s’applique pas dans cet environnement.

<a name="EM0004" />

### <a name="em0004-the-target-x-is-not-valid"></a>EM0004 : La cible `X` n’est pas valide.

L’outil ne prend pas en charge la cible `X`. Il est possible qu’une autre version de l’outil prend en charge ou qu’il ne s’applique pas dans cet environnement.

<a name="EM0005" />

### <a name="em0005-the-compilation-target-x-is-not-valid"></a>EM0005 : La cible de compilation `X` n’est pas valide.

L’outil ne prend pas en charge la cible de compilation `X`. Il est possible qu’une autre version de l’outil prend en charge ou qu’il ne s’applique pas dans cet environnement.

<a name="EM0006" />

### <a name="em0006-could-not-find-the-xcode-location"></a>EM0006 : L’emplacement de Xcode est introuvable.

L’outil n’a pas trouvé l’emplacement Xcode actuellement sélectionné en utilisant la `xcode-select -p` commande. Veuillez vérifier que cette commande réussit et retourne l’emplacement de Xcode approprié.

<a name="EM0007" />

### <a name="em0007-could-not-get-the-sdk-version-for-sdk"></a>EM0007 : Impossible d’obtenir la version du sdk pour '{sdk}'.

L’outil n’a pas pu obtenir la version de kit de développement logiciel à l’aide de la `xcrun --show-sdk-version --sdk {sdk}` commande. Veuillez vérifier que cette commande réussit et retourne la version SDK.

<a name="EM0008" />

### <a name="em0008-the-architecture-arch-is-not-valid-for-platform-valid-architectures-for-platform-are-architectures"></a>EM0008 : L’architecture '{arch}' n’est pas valide pour la plateforme {}. Les architectures valides pour la plateforme {} sont : « {architectures} ».

L’architecture dans le message d’erreur n’est pas valide pour la plateforme ciblée. Vérifiez que l’option--abi est passée à une architecture valide.

<a name="EM0009" />

### <a name="em0009-the-feature-x-is-not-currently-implemented-by-the-generator"></a>EM0009 : La fonctionnalité `X` n’est pas actuellement implémenté par le Générateur

Il s’agit d’un problème connu que nous avons l’intention de corriger dans une prochaine version du générateur. Contributions sont les bienvenus.

<a name="EM0010" />

### <a name="em0010-cant-merge-the-frameworks-simulatorframework-and-deviceframework-because-the-file-file-exists-in-both"></a>EM0010: Impossible de fusionner les infrastructures '{simulatorFramework}' et '{deviceFramework}', car le fichier ' {}' existe dans les deux.

L’outil Impossible de fusionner les frameworks mentionnés dans le message d’erreur, car il existe un fichier commun entre eux.

Cela peut indiquer un bogue dans l’incorporation de .NET ; Veuillez soumettre un rapport de bogue à [ https://github.com/mono/Embeddinator-4000/issues ](https://github.com/mono/Embeddinator-4000/issues) avec un cas de test.

<a name="EM0011" />

### <a name="em0011-the-assembly-x-does-not-exist"></a>EM0011 : L’assembly `X` n’existe pas.

L’outil n’a pas pu trouver l’assembly `X` spécifiées dans les arguments.

<a name="EM0012" />

### <a name="em0012-the-assembly-name-x-is-not-unique"></a>EM0012 : Le nom d’assembly `X` n’est pas unique

Plusieurs assemblys fournis ont le même nom interne et il ne serait pas possible de les distinguer lors de l’exécution.

La cause la plus probable est qu’un assembly est spécifié plusieurs fois sur les arguments de ligne de commande. Toutefois un conserve toujours renommé assembly il est le nom d’origine et plusieurs copies ne peut pas coexiste.

<a name="EM0013" />

### <a name="em0013-cant-find-the-assembly-x-referenced-by-y"></a>EM0013 : Impossible de trouver l’assembly 'X', référencée par « Y ».

L’assembly 'X' référencé par l’assembly 'Y', l’outil est introuvable. Vérifiez que tous les assemblys référencés sont dans le même répertoire que l’assembly à lier.

<a name="EM0014" />

### <a name="em0014-could-not-find-product-product-minversion-is-required"></a>EM0014 : N’a pas trouvé {product} ({product} {min_version} est requis).

La dépendance mentionnée dans le message d’erreur est introuvable sur le système.

<a name="EM0015" />

### <a name="em0015-could-not-find-a-valid-version-of-product-found-version-but-at-least-minversion-is-required"></a>EM0015 : Impossible de trouver une version valide de {product} (trouvé {version}, mais au moins {min_version} est requis).

La dépendance mentionné dans l’erreur message a été trouvé sur le système, mais il est trop ancien. Mettez à jour vers une version plus récente.

<a name="EM0016" />

### <a name="em0016-could-not-create-symlink-file---target-error-number"></a>EM0016 : Lien symbolique n’a pas pu être créer '{file}' -> '{'target} : erreur {number}

N’a pas pu créer le lien symbolique mentionné dans le message d’erreur.

<a name="EM0026" />

### <a name="em0026-could-not-parse-the-command-line-argument-a-"></a>EM0026 a pas pu analyser l’argument de ligne de commande « A » : *

La syntaxe pour l’option de ligne de commande `A` n’a pas pu être analysé par l’outil. Il est probable incorrectes, veuillez consulter la documentation ou l’aide de la syntaxe correcte.

<a name="EM0099" />

### <a name="em0099-internal-error--please-file-a-bug-report-with-a-test-case-httpsgithubcommonoembeddinator-4000issues"></a>EM0099 : Erreur interne *. Veuillez soumettre un rapport de bogue avec un cas de test (https://github.com/mono/Embeddinator-4000/issues).

Ce message d’erreur est signalé en cas d’échec d’une vérification de cohérence interne dans l’incorporation de .NET.

Cela indique un bogue dans l’incorporation de .NET ; Veuillez soumettre un rapport de bogue à [ https://github.com/mono/Embeddinator-4000/issues ](https://github.com/mono/Embeddinator-4000/issues) avec un cas de test.

<!-- 1xxx: code processing -->

## <a name="em1xxx-code-processing"></a>EM1xxx : Traitement de code

<a name="EM1010" />

### <a name="em1010-type-t-is-not-generated-because-x-are-not-supported"></a>EM1010 : Type `T` n’est pas généré, car `X` ne sont pas pris en charge.

Il s’agit d’un **avertissement** que le type `T` va être ignorée (par exemple, rien ne sera générée), car elle utilise `X`, une fonctionnalité qui n’est pas pris en charge.

Remarque : Fonctionnalités prises en charge évoluera avec les nouvelles versions de l’outil.

<a name="EM1011" />

### <a name="em1011-type-t-is-not-generated-because-it-lacks-marshaling-code-with-a-native-counterpart"></a>EM1011 : Type `T` n’est pas généré, car il ne dispose pas de code de marshaling avec un équivalent natif.

Il s’agit d’un **avertissement** qui le type `T` va être ignorée (par exemple, rien ne sera générée), car elle expose quelque chose à partir de .NET framework qui nécessite un marshaling supplémentaires.

Remarque : Il s’agit d’un élément qui peut obtenir pris en charge, avec certaines limitations, dans une future version de l’outil.

<a name="EM1020" />

### <a name="em1020-constructor-c-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1020 : Constructeur `C` n’est pas généré en raison du type de paramètre `T` n’est pas pris en charge.

Il s’agit d’un **avertissement** que le constructeur `C` va être ignorée (par exemple, rien ne sera générée), car un paramètre de type `T` n’est pas pris en charge.

Il doit y avoir un avertissement antérieures, ce qui donne plus d’informations pourquoi tapez `T` n’est pas pris en charge.

Remarque : Fonctionnalités prises en charge évoluera avec les nouvelles versions de l’outil.

<a name="EM1021" />

### <a name="em1021-constructor-c-has-default-values-for-which-no-wrapper-is-generated"></a>EM1021 : Constructeur `C` a des valeurs par défaut pour laquelle aucun wrapper n’est généré.

Il s’agit d’un **avertissement** que les paramètres par défaut du constructeur `C` n’établissent pas de code supplémentaire. La cause la plus courante est qu’une méthode existante a déjà la même signature. Par exemple, dans .net, il est possible d’avoir :

```
public class MyType {
    public MyType () { ... }
    public MyType (int i = 0) { ... }
}
```

Dans ce cas seulement deux généré `init` sélecteurs seront créés, un appel à la fois dans Mono, mais aucun wrapper pour les versions ultérieures n’existera.

<a name="EM1030" />

### <a name="em1030-method-m-is-not-generated-because-return-type-t-is-not-supported"></a>EM1030 : Méthode `M` n’est pas générée comme type de retour `T` n’est pas pris en charge.

Il s’agit d’un **avertissement** qui la méthode `M` va être ignorée (par exemple, rien ne sera générée), car il est de type de retour `T` n’est pas pris en charge.

Il doit y avoir un avertissement antérieures, ce qui donne plus d’informations pourquoi tapez `T` n’est pas pris en charge.

Remarque : Fonctionnalités prises en charge évoluera avec les nouvelles versions de l’outil.

<a name="EM1031" />

### <a name="em1031-method-m-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1031 : Méthode `M` n’est pas généré en raison du type de paramètre `T` n’est pas pris en charge.

Il s’agit d’un **avertissement** que la méthode `M` va être ignorée (par exemple, rien ne sera générée), car un paramètre de type `T` n’est pas pris en charge.

Il doit y avoir un avertissement antérieures, ce qui donne plus d’informations pourquoi tapez `T` n’est pas pris en charge.

Remarque : Fonctionnalités prises en charge évoluera avec les nouvelles versions de l’outil.

<a name="EM1032" />

### <a name="em1032-method-m-has-default-values-for-which-no-wrapper-is-generated"></a>EM1032 : Méthode `M` a des valeurs par défaut pour laquelle aucun wrapper n’est généré.

Il s’agit d’un **avertissement** que les paramètres par défaut de la méthode `M` n’établissent pas de code supplémentaire. La cause la plus courante est qu’une méthode existante a déjà la même signature. Par exemple, dans .net, il est possible d’avoir :

```
public class MyType {
    public int Increment () { ... }
    public int Increment (int i = 0) { ... }
}
```

Dans ce cas seulement deux généré `increment` sélecteurs seront créés, un appel à la fois dans Mono, mais aucun wrapper pour les versions ultérieures n’existera.

<a name="EM1033" />

### <a name="em1033-method-m-is-not-generated-because-another-method-exposes-the-operator-with-a-friendly-name"></a>EM1033 : Méthode `M` n’est pas généré, car une autre méthode expose l’opérateur avec un nom convivial.

Il s’agit d’un **avertissement** que la méthode `M` n’est pas généré, car une autre méthode expose l’opérateur avec un nom convivial. (https://msdn.microsoft.com/library/ms229032(v=vs.110).aspx)

<a name="EM1034" />

### <a name="em1034-extension-method-m-is-not-generated-inside-a-category-because-they-cannot-be-created-on-primitive-type-t-a-normal-static-method-was-generated"></a>EM1034 : Méthode d’extension `M` n’est pas généré à l’intérieur d’une catégorie, car ils ne peuvent pas être créés sur le type primitif `T`. Une méthode statique normale a été générée.

Il s’agit d’un **avertissement** que le type de méthode d’extension sur un primivite (par exemple, `System.Int32`) a été trouvé. En Objective-C, il n’est pas possible de créer des catégories sur le type primitif. Au lieu de cela le générateur être produira une méthode statique normale.

<a name="EM1040" />

### <a name="em1040-property-p-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1040 : Propriété `P` n’est pas généré en raison du type de paramètre `T` n’est pas pris en charge.

Il s’agit d’un **avertissement** qui la propriété `P` va être ignorée (par exemple, rien ne sera générée), car le type exposé `T` n’est pas pris en charge.

Il doit y avoir un avertissement antérieures, ce qui donne plus d’informations pourquoi tapez `T` n’est pas pris en charge.

Remarque : Fonctionnalités prises en charge évoluera avec les nouvelles versions de l’outil.

<a name="EM1041" />

### <a name="em1041-indexed-properties-on-t-is-not-generated-because-multiple-indexed-properties-are-not-supported"></a>EM1041 : Propriétés indexées sur `T` n’est pas généré, car plusieurs propriétés indexées ne sont pas pris en charge.

Il s’agit d’un **avertissement** qui les propriétés indexées sur `T` va être ignorée (par exemple, rien ne sera générée), car plusieurs propriétés indexées ne sont pas pris en charge.

<a name="EM1050" />

### <a name="em1050-field-f-is-not-generated-because-of-field-type-t-is-not-supported"></a>EM1050 : Champ `F` n’est pas généré en raison du type de champ `T` n’est pas pris en charge.

Il s’agit d’un **avertissement** qui le champ `F` va être ignorée (par exemple, rien ne sera générée), car le type exposé `T` n’est pas pris en charge.

Il doit y avoir un avertissement antérieures, ce qui donne plus d’informations pourquoi tapez `T` n’est pas pris en charge.

Remarque : Fonctionnalités prises en charge évoluera avec les nouvelles versions de l’outil.

<a name="EM1051" />

### <a name="em1051-element-e-is-generated-instead-as-f-because-its-name-conflicts-with-an-important-objective-c-selector"></a>EM1051 : Élément `E` est généré à la place comme `F` car son nom est en conflit avec un sélecteur d’objective-c important.

Il s’agit d’un **avertissement** qui l’élément `E` va être généré en tant que `F` car son nom est en conflit avec un sélecteur d’objective-c important.

Sélecteurs sur le [NSObjectProtocol](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc) ont une signification importante en objective-c et doit être substitué avec soin.

Remarque : La liste des sélecteurs réservées évoluera avec les nouvelles versions de l’outil.

<a name="EM1052" />

### <a name="em1052-element-e-is-not-generated-its-name-conflicts-with-other-elements-on-the-same-class"></a>EM1052 : Élément `E` n’est pas généré son nom est en conflit avec d’autres éléments sur la même classe.

Il s’agit d’un **avertissement** cet élément `E` n’est pas générée comme son nom est en conflit avec d’autres éléments sur la même classe.

<a name="EM1053" />

### <a name="em1053-target-e-is-not-supported-for-xamarinios-and-xamarinmac-only-the-framework-option-is-considered-supported-and-should-be-used"></a>EM1053 : Cible `E` n’est pas pris en charge pour Xamarin.iOS et Xamarin.Mac. Uniquement la `framework` option est considérée comme pris en charge et doit être utilisé.

Il s’agit d’un **avertissement** qui ciblent `E` est considéré comme non pris en charge pour Xamarin.iOS et Xamarin.Mac cas d’usage. 

Consommation des bibliothèques statiques ou dynamiques incorporation .NET peut-être nécessiter des étapes de travail supplémentaires ou des ajustements et doit être évitée dans la plupart des cas d’utilisation.

Envisagez de supprimer votre `--target` paramètre ou passer `--target=framework` à la place.

<!-- 2xxx: code generation -->

## <a name="em2xxx-code-generation"></a>EM2xxx : Génération de code

<!-- 3xxx: reserved -->
<!-- 4xxx: reserved -->
<!-- 5xxx: reserved -->
<!-- 6xxx: reserved -->
<!-- 7xxx: reserved -->
<!-- 8xxx: reserved -->
<!-- 9xxx: reserved -->
