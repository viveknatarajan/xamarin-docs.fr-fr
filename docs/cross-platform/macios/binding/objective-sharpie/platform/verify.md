---
title: Vérifiez les attributs
ms.prod: xamarin
ms.assetid: 107FBCEA-266B-4295-B7AA-40A881B82B7B
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: b9409b4351ed9233db0edf8e2dd9f516b9727fe0
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="verify-attributes"></a>Vérifiez les attributs


Vous constaterez souvent que les liaisons produits par objectif Sharpie seront annotés avec le `[Verify]` attribut. Ces attributs indiquent que vous devez _vérifier_ qu’objectif Sharpie a le mieux en comparant la liaison avec la déclaration de C/Objective-C d’origine (qui sera fournie dans un commentaire au-dessus de la déclaration lié).

Vérification est recommandée pour _tous les_ lié déclarations, mais est plus probable _requis_ pour les déclarations annotées avec le `[Verify]` attribut. Il s’agit, car dans de nombreuses situations, il n’est pas suffisamment de métadonnées dans le code natif source d’origine pour déduire comment mieux produire une liaison. Vous devrez peut-être référencer documentation ou des commentaires de code dans les fichiers d’en-tête à prendre la meilleure décision de liaison.

Une fois que vous avez vérifié que la liaison est corriger ou avez fixe pour être correct, _supprimer_ le `[Verify]` attribut à partir de la liaison.

> [!IMPORTANT]
> `[Verify]` attributs intentionnellement les erreurs de compilation c# afin que vous êtes obligé de vérifier la liaison. Vous devez supprimer la `[Verify]` lorsque vous avez consulté (et éventuellement corrigé) le code d’attribut.

## <a name="verify-hints-reference"></a>Vérifier la référence d’indicateurs

L’argument de l’indicateur fourni à l’attribut peut faire l’objet référencé avec la documentation ci-dessous. Documentation pour les produits `[Verify]` attributs seront fournies sur la console ainsi la fin de la liaison.

|Vérifiez l’indicateur|Description|
|---|---|
|InferredFromPreceedingTypedef|Le nom de cette déclaration a été inféré par convention commune à partir de la précédant immédiatement `typedef` dans le code natif source d’origine. Vérifiez que le nom déduit est correct de cette convention est AMBIGUE.|
|ConstantsInterfaceAssociation|Il n’existe aucun moyen de très sûr pour déterminer quelle interface Objective-C une déclaration de variable externe peut être associée. Les instances de ces sont liées en tant que `[Field]` propriétés dans une interface partielle dans une interface concrète près par pour produire une API intuitive, éventuellement en éliminant les constantes' ' interface complètement.|
|MethodToProperty|Une méthode Objective-C a été liée en tant que propriété c# en raison d’une convention tels que les prenant pas de paramètres et retournant une valeur (retour non void). Souvent des méthodes comme il doivent être liés en tant que propriétés à exposer une API agréable, mais des faux positifs peuvent parfois se produire et la liaison doit être en fait une méthode.|
|StronglyTypedNSArray|Natif `NSArray*` a été liées en tant que `NSObject[]`. Il est possible pour un type plus fort du tableau dans la liaison basée sur les attentes définies via la documentation de l’API (par exemple, les commentaires dans le fichier d’en-tête) ou en examinant le contenu du tableau et de test. Par exemple, un NSArray * contenant uniquement NSNumber * instancescan être liées en tant que `NSNumber[]` au lieu de `NSObject[]`.|

Vous pouvez recevoir rapidement de documentation pour un indicateur à l’aide du `sharpie verify-docs` outil, par exemple :

```csharp
sharpie verify-docs InferredFromPreceedingTypedef
```

