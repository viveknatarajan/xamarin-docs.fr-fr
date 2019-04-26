---
title: Historique des versions de Sharpie objectif
description: Ce document décrit l’historique de publication de Sharpie objectif, l’outil utilisé pour automatiser la création de C# liaisons au code Objective-C.
ms.prod: xamarin
ms.assetid: 1F4A1BE1-7205-43F4-89D0-6C8672F52598
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: 03e4a5ac8906d2593cbdf3c15f6b2d1f4a2c6d19
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61199647"
---
# <a name="objective-sharpie-release-history"></a>Historique des versions de Sharpie objectif

## <a name="34-october-11-2017"></a>3.4 (11 octobre 2017)

[Télécharger v3.4.0](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie-3.4.0.pkg)

* Prise en charge de Xcode 9 : iOS 11, macOS 10.13, 11, de tvOS et watchOS 4
* Problèmes avec SIMD et tgmath doivent maintenant être résolus
* Données de télémétrie a été complètement supprimée

## <a name="33-august-3-2016"></a>3.3 (3 août 2016)

[Télécharger v3.3.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.3.0.pkg)

* Prise en charge pour Xcode 8 bêta 4, iOS 10, macOS 10.12, tvOS 10 et watchOS 3.
* Mise à jour pour la dernière Clang génération maître (2016-08-02)
* [Conserver les options d’envoi de données de télémétrie](https://twitter.com/Symbiatch/status/760373403878559744) de `sharpie pod bind` à `sharpie bind`.

## <a name="32-june-14-2016"></a>3.2 (14 juin 2016)

[Télécharger v3.2.3](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.2.3.pkg)

* Prendre en charge pour la version bêta 1 de Xcode 8, iOS 10, macOS 10.12, tvOS 10 et watchOS 3.

## <a name="31-may-31-2016"></a>3.1 (31 mai 2016)

[Télécharger v3.1.1](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.1.1.pkg)

* Prise en charge de CocoaPods 1.0
* Meilleure fiabilité de liaison de CocoaPods en premier générant native `.framework` puis la liaison qui
* Copier les CocoaPods `.framework` et liaison de définition dans un `Binding` directory pour faciliter l’intégration avec les projets de liaison Xamarin.iOS et Xamarin.Mac

## <a name="30-october-5-2015"></a>3.0 (5 octobre 2015)

[Télécharger v3.0.8](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.0.8.pkg)

* Prise en charge des nouvelles fonctionnalités de langage Objective-C, y compris des génériques légers et possibilité de valeur null, introduite dans Xcode 7
* Prise en charge pour les dernières iOS et kits de développement logiciel Mac.
* Projet Xcode création et l’analyse de prise en charge ! Vous pouvez maintenant passer à un projet Xcode complet pour objectif Sharpie et elle s’efforcera de déterminer la bonne chose à faire (par exemple, `sharpie bind Project.xcodeproj -sdk ios`).
* [CocoaPods](https://cocoapods.org) prennent en charge ! Vous pouvez maintenant configurer, générer et lier les CocoaPods directement à partir de l’objectif Sharpie (par exemple, `sharpie pod init ios AFNetworking && sharpie pod bind`).
* Lors de la liaison d’infrastructures, les modules Clang seront activés si le framework prend en charge, ce qui entraîne le mieux adapté d’analyse d’une infrastructure, étant donné que la structure du framework est définie par son `module.modulemap`.
* Pour les projets Xcode qui génèrent un produit de framework, analyser ce produit au lieu de cibles de produit intermédiaire comme cibles non framework dans un projet Xcode ait des ambiguïtés qui ne peut pas être résolues automatiquement.

## <a name="216-march-17-2015"></a>2.1.6 (17 mars 2015)

[Télécharger v2.1.6](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.6.pkg)

* Liaison d’expression opérateur binaire fixe : la partie gauche de l’expression a été incorrectement échanger avec la droite (par exemple, `1 << 0` a été incorrectement liées en tant que `0 << 1`). Merci à Adam Kemp pour constaté que cela !
* Correction d’un problème avec `NSInteger` et `NSUInteger` liée en tant que `int` et `uint` au lieu de `nint` et `nuint` sur i386 ; `-DNS_BUILD_32_LIKE_64` est maintenant passé à Clang pour rendre l’analyse `objc/NSObjCRuntime.h` fonctionne comme prévu sur i386.
* L’architecture par défaut pour les kits de développement logiciel Mac OS X (par exemple, `-sdk macosx10.10`) est désormais x86_64 au lieu d’i386, par conséquent, `-arch` peut être omis, sauf si vous souhaitez substituer la valeur par défaut.

## <a name="210-march-15-2015"></a>2.1.0 (15 mars 2015)

[Télécharger v2.1.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.0.pkg)

* [bxc#27849](https://bugzilla.xamarin.com/show_bug.cgi?id=27849): Vérifiez `using ObjCRuntime;` est généré lorsque `ArgumentSemantic` est utilisé.
* [bxc#27850](https://bugzilla.xamarin.com/show_bug.cgi?id=27850): Vérifiez `using System.Runtime.InteropServices;` est généré lorsque `DllImport` est utilisé.
* [bxc#27852](https://bugzilla.xamarin.com/show_bug.cgi?id=27852): Par défaut `DllImport` au chargement de symboles à partir de `__Internal`.
* [bxc#27848](https://bugzilla.xamarin.com/show_bug.cgi?id=27848): Ignorer les déclarations de conteneur Objective-C-déclaré avant.
* [bxc#27846](https://bugzilla.xamarin.com/show_bug.cgi?id=27846): Lier les types de protocole avec une qualification unique en tant qu’interfaces concrètes (`id<Foo>` comme `Foo` au lieu de `Foundation.NSObject<Foo>`).
* [bxc#28037](https://bugzilla.xamarin.com/show_bug.cgi?id=28037): Lier `UInt32`, `UInt64`, et `Int64` littéraux en tant que `Int32` pour supprimer la `u` et/ou `uL` suffixes lorsque les valeurs s’adapte en toute sécurité à `Int32`.
* [bxc#28038](https://bugzilla.xamarin.com/show_bug.cgi?id=28038): Corriger le mappage de noms d’enum lorsque le nom natif d’origine commence par un `k` préfixe.
* `sizeof` Les expressions C dont le type d’argument n’est pas mappé à un C# type primitif sera évalué dans Clang et lié comme un littéral d’entier pour éviter de générer non valide C#.
* Corrigez la syntaxe Objective-C pour les propriétés dont le type est un bloc (code Objective-C apparaît dans les commentaires au-dessus des déclarations de liées).
* Lier les types de cariés de leur type d’origine (`int[]` chute à `int*` pendant l’analyse sémantique dans Clang, mais lier en tant que l’original comme écrit par `int[]` à la place).

Merci beaucoup pour Dave Dunkin pour la plupart des bogues résolus dans cette version de point de reporting.

## <a name="200-march-9-2015"></a>2.0.0: 9 mars 2015

[Télécharger la version 2.0.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.0.0.pkg)

Objectif Sharpie 2.0 est une version majeure qui comprend un pilote Clang améliorée et analyseur et un nouveau moteur de liaison en fonction du NRefactory. Ces composants améliorées fournissent pour _beaucoup_ mieux les liaisons, notamment concernant la liaison de type. De nombreuses autres améliorations ont été apportées qui sont internes à Sharpie objectif qui assureront de nombreuses fonctionnalités visible par l’utilisateur dans les versions futures.

Objectif Sharpie 2.0 est basé sur Clang 3.6.1.

### <a name="type-binding-improvements"></a>Améliorations de liaison de type

* Objective-C blocs sont désormais pris en charge. Cela inclut les blocs d’anonyme/inline et blocs nommés `typedef`. Blocs anonymes seront liés en tant que `System.Action` ou `System.Func` délègue, tandis que les blocs nommés seront liés comme un nom fort `delegate` types.

* Il existe une heuristique améliorée d’affectation de noms pour les enums anonymes qui sont immédiatement précédés d’un `typedef` résolution à un type intégral builtin comme `long` ou `int`.

* Les pointeurs C sont maintenant liées en tant que C# `unsafe` des pointeurs plutôt `System.IntPtr`. Il en résulte dans plus de clarté dans la liaison pour lorsque vous souhaitez activer les paramètres de pointeur dans `out` ou `ref` paramètres. Il n’est pas possible de toujours déduire si un paramètre doit être `out` ou `ref`, de sorte que le pointeur est conservé dans la liaison pour permettre l’audit plus facile.

* Une exception à la liaison de pointeur ci-dessus est lorsqu’un pointeur de rang de 2 à un objet Objective-C est rencontré en tant que paramètre. Dans ce cas, la convention est prédominante et le paramètre sera lié en tant que `out` (par exemple, `NSError **error` → `out NSError error`).

### <a name="verify-attribute"></a>Vérifier l’attribut

Vous trouverez souvent que les liaisons produites par objectif Sharpie seront désormais annotées avec le `[Verify]` attribut. Ces attributs indiquent que vous devez _vérifier_ qu’objectif Sharpie fait la chose correcte en comparant la liaison avec la déclaration d’origine de C/Objective-C (qui vous sont fournie dans un commentaire au-dessus de la déclaration de liaison).

La vérification est _recommandé_ toutes les déclarations de liées, mais est plus probable _requis_ pour les déclarations annotées avec le `[Verify]` attribut. Il s’agit, car dans de nombreuses situations, il n’est pas suffisamment de métadonnées dans le code natif source d’origine pour déduire comment mieux produire une liaison. Vous devrez peut-être faire référence à la documentation ou des commentaires de code dans les fichiers d’en-tête à prendre la bonne décision de liaison.

Consultez le [vérifier les attributs](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md) documentation pour plus d’informations.

### <a name="other-notable-improvements"></a>Autres améliorations notables

* `using` les instructions sont maintenant générées selon les types liés. Par exemple, si un `NSURL` a été liée, un `using Foundation;` instruction sera également générée.

* `struct` et `union` déclarations va maintenant être liées, à l’aide de la `[FieldOffset]` astuce pour les unions.

* Valeurs d’énumération avec les initialiseurs de l’expression constante seront désormais correctement liés ; l’expression complète est traduite en C#.

* Blocs et les méthodes Variadic sont maintenant liées.

* Infrastructures sont désormais pris en charge par le biais de la `-framework` option. Consultez la documentation sur [liaison d’infrastructures natives](https://developer.xamarin.com/guides/ios/advanced_topics/binding_objective-c/objective_sharpie/#frameworks) pour plus d’informations.

* Code objective-C source sera détecté automatiquement à présent, ce qui devrait éliminer la nécessité de passer `-ObjC` ou `-xobjective-c` pour Clang manuellement.

* Clang l’utilisation des modules (`@import`) est désormais détecté automatiquement, ce qui devrait éliminer la nécessité de passer `-fmodules` pour Clang manuellement pour les bibliothèques qui utilisent la nouvelle prise en charge du module dans Clang.

* L’API unifiée de Xamarin est désormais la cible de liaison par défaut ; Utilisez la `-classic` option pour cibler l’API classique de 32 bits uniquement.

### <a name="notable-bug-fixes"></a>Correctifs de bogues notables

* Corriger `instancetype` liaison lorsqu’il est utilisé dans une catégorie Objective-C
* Catégories de Objective-C de nom complet
* Préfixe des protocoles Objective-C avec `I` (par exemple, `INSCopying` au lieu de `NSCopying`)

## <a name="1135-december-21-2014"></a>1.1.35: 21 décembre 2014

[Télécharger v1.1.35](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.35.pkg)

Correctifs de bogues mineurs.

## <a name="111-december-15-2014"></a>1.1.1: 15 décembre 2014

[Télécharger la version 1.1.1](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.1.pkg)

1.1.1 a été la première version majeure après 1,5 années d’utilisation interne et de développement chez Xamarin suivant la version préliminaire initiale de l’objectif Sharpie en avril 2013. Cette version est le premier à être généralement considérées comme stables et utilisable pour une grande variété de bibliothèques natives, avec un serveur principal Clang.

