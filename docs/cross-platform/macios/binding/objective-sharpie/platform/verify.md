---
title: Objectif Sharpie vérifier les attributs
description: Ce document décrit l’attribut [Verify] généré par objectif Sharpie. L’attribut [Verify] met en surbrillance pour les développeurs où ils doivent vérifier manuellement les sortie de Sharpie objectif.
ms.prod: xamarin
ms.assetid: 107FBCEA-266B-4295-B7AA-40A881B82B7B
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 4bca896afb4dfc96fd6c1d7cdf489feb6a879e31
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855027"
---
# <a name="objective-sharpie-verify-attributes"></a>Objectif Sharpie vérifier les attributs

Vous trouverez souvent que les liaisons produites par objectif Sharpie seront annotées avec le `[Verify]` attribut. Ces attributs indiquent que vous devez _vérifier_ qu’objectif Sharpie fait la chose correcte en comparant la liaison avec la déclaration d’origine de C/Objective-C (qui vous sont fournie dans un commentaire au-dessus de la déclaration de liaison).

Vérification est recommandée pour _tous les_ lié les déclarations, mais est probablement _requis_ pour les déclarations annotées avec le `[Verify]` attribut. Il s’agit, car dans de nombreuses situations, il n’est pas suffisamment de métadonnées dans le code natif source d’origine pour déduire comment mieux produire une liaison. Vous devrez peut-être faire référence à la documentation ou des commentaires de code dans les fichiers d’en-tête à prendre la bonne décision de liaison.

Une fois que vous avez vérifié que la liaison est corriger ou pour qu’il soit correct, rectifiées _supprimer_ le `[Verify]` attribut à partir de la liaison.

> [!IMPORTANT]
> `[Verify]` attributs intentionnellement les erreurs de compilation c# afin que vous êtes obligé de vérifier la liaison. Vous devez supprimer le `[Verify]` lorsque vous avez consulté (et éventuellement corrigé) le code d’attribut.

## <a name="verify-hints-reference"></a>Vérifier la référence d’indicateurs

L’argument de l’indicateur fourni à l’attribut peut faire l’objet référencé avec la documentation ci-dessous. Documentation de tout produit `[Verify]` attributs seront fournies ainsi la console une fois la liaison terminée.

|`[Verify]` Indicateur|Description|
|---|---|
|InferredFromPreceedingTypedef|Le nom de cette déclaration a été inféré par convention commune à partir de la précédant immédiatement `typedef` dans le code natif source d’origine. Vérifiez que le nom déduit est correct de cette convention est AMBIGUE.|
|ConstantsInterfaceAssociation|Il n’existe aucun moyen de preuve de poisson pour déterminer quelle interface Objective-C une déclaration de variable extern peut être associée. Instances de ces sont liées en tant que `[Field]` propriétés dans une interface partielle dans une interface concrète près par pour produire une API plus intuitive, éventuellement éliminer les « constantes » de l’interface complètement.|
|MethodToProperty|Une méthode Objective-C a été liée en tant que propriété c# en raison d’une convention comme prenant pas de paramètres et retournant une valeur (retour non void). Souvent les méthodes comme ceux-ci doivent être liés en tant que propriétés d’exposer une API mieux, mais parfois, les faux positifs peuvent se produire et la liaison doit être en fait une méthode.|
|StronglyTypedNSArray|Native `NSArray*` a été liées en tant que `NSObject[]`. Il peut être possible de taper plus fortement le tableau dans la liaison selon les attentes définie via la documentation de l’API (par exemple, les commentaires dans le fichier d’en-tête) ou en examinant le contenu du tableau et de test. Par exemple, un NSArray * contenant uniquement NSNumber * instancescan être lié comme `NSNumber[]` au lieu de `NSObject[]`.|

Vous pouvez recevoir également rapidement de documentation pour un indicateur à l’aide de la `sharpie verify-docs` de l’outil, par exemple :

```csharp
sharpie verify-docs InferredFromPreceedingTypedef
```

## <a name="related-links"></a>Liens associés

- [Cours de l’Université de Xamarin : Génération d’une bibliothèque de liaisons Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University cours : Générer une bibliothèque de liaisons Objective-C avec Sharpie objectif](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
