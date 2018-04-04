---
title: Historique des versions
ms.prod: xamarin
ms.assetid: 1F4A1BE1-7205-43F4-89D0-6C8672F52598
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: a9eb4b1ea958f2756eb2a21ee44d0d0dfcf8b931
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="release-history"></a>Historique des versions

## <a name="34-october-11-2017"></a>3.4 (11 octobre 2017)

[Télécharger v3.4.0](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie-3.4.0.pkg)

* Prise en charge de Xcode 9 : iOS, 11, macOS 10.13, tvOS 11 et watchOS 4
* Problèmes avec SIMD et tgmath doivent maintenant être résolus
* Données de télémétrie a été supprimée complètement

## <a name="33-august-3-2016"></a>3.3 (3 août 2016)

[Télécharger v3.3.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.3.0.pkg)

* Prise en charge Xcode 8 bêta 4, iOS 10, macOS 10.12, tvOS 10 et watchOS 3.
* Mise à jour pour la dernière Clang génération maître (2016-08-02)
* [Conserver les options d’envoi de données de télémétrie](https://twitter.com/Symbiatch/status/760373403878559744) de `sharpie pod bind` à `sharpie bind`.

## <a name="32-june-14-2016"></a>3.2 (14 juin 2016)

[Télécharger v3.2.3](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.2.3.pkg)

* Prise en charge Xcode 8 bêta 1, iOS 10, macOS 10.12, tvOS 10 et watchOS 3.

## <a name="31-may-31-2016"></a>3.1 (31 mai 2016)

[Télécharger v3.1.1](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.1.1.pkg)

* Prise en charge de CocoaPods 1.0
* Amélioration de la fiabilité de liaison CocoaPods par la première génération natif `.framework` puis la liaison qui
* Copiez CocoaPods `.framework` et liaison de définition dans un `Binding` répertoire afin de faciliter l’intégration avec les projets de liaison Xamarin.iOS et Xamarin.Mac

## <a name="30-october-5-2015"></a>3.0 (5 octobre 2015)

[Télécharger v3.0.8](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.0.8.pkg)

* Prise en charge des nouvelles fonctionnalités de langage Objective-C, y compris des génériques légers et la possibilité de valeur null, introduite dans Xcode 7
* Prise en charge les e/s et les kits de développement logiciel Mac plus récents.
* Projet de Xcode génération et l’analyse de prise en charge ! Vous pouvez à présent passer un projet Xcode complète à l’objectif Sharpie et il fera de son mieux pour déterminer la meilleure (par exemple, `sharpie bind Project.xcodeproj -sdk ios`).
* [CocoaPods](https://cocoapods.org) prennent en charge ! Vous pouvez maintenant configurer, générer et lier CocoaPods directement à partir de l’objectif Sharpie (par exemple, `sharpie pod init ios AFNetworking && sharpie pod bind`).
* Lors de la liaison des infrastructures, les modules Clang seront activés si le framework prend en charge, ce qui entraîne l’analyse plus approprié d’une infrastructure, étant donné que la structure de l’infrastructure est définie par son `module.modulemap`.
* Pour les projets Xcode qui génèrent un produit, d’analyser ce produit à la place des cibles de produit intermédiaire comme cibles non framework dans un projet Xcode ait des ambiguïtés qui ne peut pas être résolues automatiquement.

## <a name="216-march-17-2015"></a>2.1.6 (17 mars 2015)

[Télécharger v2.1.6](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.6.pkg)

* Liaison de l’expression opérateur binaire fixe : la partie gauche de l’expression a été incorrectement échangée avec le droit (par exemple, `1 << 0` a été incorrectement liées en tant que `0 << 1`). Merci pour Adam Kemp pour cette détection.
* Correction d’un problème avec `NSInteger` et `NSUInteger` liée en tant que `int` et `uint` au lieu de `nint` et `nuint` sur i386 ; `-DNS_BUILD_32_LIKE_64` est maintenant passé à Clang pour rendre l’analyse `objc/NSObjCRuntime.h` fonctionne comme prévu sur i386.
* L’architecture par défaut pour les kits de développement logiciel Mac OS X (par exemple, `-sdk macosx10.10`) est désormais x86_64 au lieu d’i386, par conséquent, `-arch` peut être omis, sauf si la substitution de la valeur par défaut est souhaitée.

## <a name="210-march-15-2015"></a>2.1.0 (15 mars 2015)

[Télécharger v2.1.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.0.pkg)

* [bxc #27849](https://bugzilla.xamarin.com/show_bug.cgi?id=27849): Assurez-vous `using ObjCRuntime;` est généré lorsque `ArgumentSemantic` est utilisé.
* [bxc #27850](https://bugzilla.xamarin.com/show_bug.cgi?id=27850): Assurez-vous `using System.Runtime.InteropServices;` est généré lorsque `DllImport` est utilisé.
* [bxc #27852](https://bugzilla.xamarin.com/show_bug.cgi?id=27852): par défaut `DllImport` pour le chargement de symboles à partir de `__Internal`.
* [bxc #27848](https://bugzilla.xamarin.com/show_bug.cgi?id=27848): ignorer des déclarations de conteneur Objective-C-déclaré avant.
* [bxc #27846](https://bugzilla.xamarin.com/show_bug.cgi?id=27846): lier des types de protocole avec une qualification unique en tant qu’interfaces concrètes (`id<Foo>` en tant que `Foo` au lieu de `Foundation.NSObject<Foo>`).
* [bxc #28037](https://bugzilla.xamarin.com/show_bug.cgi?id=28037): lier `UInt32`, `UInt64`, et `Int64` littéraux en tant que `Int32` pour supprimer la `u` et/ou `uL` suffixes lorsque les valeurs peuvent s’intégrer en toute sécurité `Int32`.
* [bxc #28038](https://bugzilla.xamarin.com/show_bug.cgi?id=28038): corriger le mappage de noms d’enum lorsque le nom d’origine natif commence par un `k` préfixe.
* `sizeof` Les expressions C dont le type d’argument ne correspond pas à un type primitif c# seront évaluées dans Clang et liées comme un littéral d’entier pour éviter de générer c# non valide.
* Corrigez la syntaxe Objective-C pour les propriétés dont le type est un bloc (code Objective-C apparaît dans les commentaires au-dessus des déclarations liées).
* Lier des types cariés de leur type d’origine (`int[]` chute à `int*` pendant l’analyse sémantique dans Clang, mais la lier en tant que l’original écrits en tant que `int[]` à la place).

Merci beaucoup pour Dave Dunkin grand nombre de bogues corrigés dans cette version de point de création de rapports.

## <a name="200-march-9-2015"></a>2.0.0 : le 9 mars 2015.

[Télécharger v2.0.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.0.0.pkg)

Objectif Sharpie 2.0 est une version majeure qui propose un pilote Clang améliorée et analyseur et un nouveau moteur liaison NRefactory. Ces composants améliorées fournissent pour _beaucoup_ mieux les liaisons, notamment autour de liaison de type. D’autres améliorations ont été apportées qui sont internes à Sharpie objectif qui retournera les nombreuses fonctionnalités visible par l’utilisateur dans les versions futures.

Objectif Sharpie 2.0 repose sur Clang 3.6.1.

### <a name="type-binding-improvements"></a>Améliorations de la liaison type

* Objective-C blocs sont désormais pris en charge. Cela inclut les blocs anonyme/inline et les blocs nommés `typedef`. Blocs anonymes seront liés en tant que `System.Action` ou `System.Func` délégués, tandis que les blocs nommés seront liés comme un nom fort `delegate` types.

* Il existe une heuristique améliorée d’affectation de noms pour les enums anonymes qui sont immédiatement précédés d’un `typedef` résolution vers un type intégral intégré tel que `long` ou `int`.

* Les pointeurs C sont maintenant liées en tant que C# `unsafe` des pointeurs plutôt `System.IntPtr`. Cela entraîne plus de clarté dans la liaison lorsque vous souhaitez activer les paramètres de pointeur dans `out` ou `ref` paramètres. Il n’est pas possible de toujours déduire si un paramètre doit être `out` ou `ref`, de sorte que le pointeur est conservé dans la liaison pour permettre plus facile d’audit.

* Une exception à la liaison de pointeur ci-dessus est lorsqu’un pointeur de rang de 2 à un objet Objective-C est rencontré en tant que paramètre. Dans ces cas, la convention est prédominante et le paramètre sera lié en tant que `out` (par exemple, `NSError **error` → `out NSError error`).

### <a name="verify-attribute"></a>Vérifier l’attribut

Vous constaterez souvent que les liaisons produits par objectif Sharpie seront désormais annotées avec le `[Verify]` attribut. Ces attributs indiquent que vous devez _vérifier_ qu’objectif Sharpie a le mieux en comparant la liaison avec la déclaration de C/Objective-C d’origine (qui sera fournie dans un commentaire au-dessus de la déclaration lié).

La vérification est _recommandé_ pour toutes les déclarations liées, mais est sans doute _requis_ pour les déclarations annotées avec le `[Verify]` attribut. Il s’agit, car dans de nombreuses situations, il n’est pas suffisamment de métadonnées dans le code natif source d’origine pour déduire comment mieux produire une liaison. Vous devrez peut-être référencer documentation ou des commentaires de code dans les fichiers d’en-tête à prendre la meilleure décision de liaison.

Consultez le [vérifier les attributs](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md) documentation pour plus de détails.

### <a name="other-notable-improvements"></a>Autres améliorations notables

* `using` les instructions sont désormais générées selon les types liés. Par exemple, si un `NSURL` a été liée, un `using Foundation;` instruction sera également générée.

* `struct` et `union` déclarations va maintenant être liées, à l’aide de la `[FieldOffset]` pli pour les unions.

* Les valeurs enum avec des initialiseurs d’expression constante seront désormais correctement liés ; l’expression complète est traduite en c#.

* Les blocs et les méthodes de Variadiques sont maintenant liées.

* Infrastructures sont désormais pris en charge la `-framework` option. Consultez la documentation sur [infrastructures natif de liaison](http://developer.xamarin.com/guides/ios/advanced_topics/binding_objective-c/objective_sharpie/#frameworks) pour plus d’informations.

* Code objective-C source sera détecté automatiquement maintenant, ce qui devrait éliminer la nécessité de passer `-ObjC` ou `-xobjective-c` pour Clang manuellement.

* Clang l’utilisation des modules (`@import`) est désormais détecté automatiquement, ce qui devrait éliminer la nécessité de passer `-fmodules` pour Clang manuellement pour les bibliothèques qui utilisent la nouvelle prise en charge du module dans Clang.

* L’API unifiée de Xamarin est désormais la cible de liaison par défaut. Utilisez la `-classic` option pour cibler l’API classique de 32 bits uniquement.

### <a name="notable-bug-fixes"></a>Correctifs de bogues importants

* Corriger `instancetype` liaison lorsqu’il est utilisé dans une catégorie Objective-C
* Catégories de Objective-C de nom complet
* Préfixe de protocoles Objective-C avec `I` (par exemple, `INSCopying` au lieu de `NSCopying`)

## <a name="1135-december-21-2014"></a>1.1.35 : 21 décembre 2014

[Télécharger v1.1.35](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.35.pkg)

Correctifs mineurs.

## <a name="111-december-15-2014"></a>1.1.1 : 15 décembre 2014

[Télécharger la version 1.1.1](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.1.pkg)

1.1.1 était la première version majeure après 1,5 années d’utilisation interne et de développement Xamarin suivant l’aperçu initial de l’objectif Sharpie en avril 2013. Cette version est le premier à être généralement considéré comme stable et utilisable pour un large éventail de bibliothèques natives, avec un serveur principal Clang.

