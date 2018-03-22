---
title: "Utilisation des valeurs par défaut de l’utilisateur"
description: "Cet article traite de travailler avec NSUserDefault pour enregistrer les paramètres par défaut dans un Xamarin iOS application ou Extension."
ms.topic: article
ms.prod: xamarin
ms.assetid: DAE7FFC4-B8C9-4D9E-886A-9B2388452EEB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: 73f3beb87fffcb37ef3e36d54f634c3bc62da538
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="working-with-user-defaults"></a>Utilisation des valeurs par défaut de l’utilisateur

_Cet article décrit l’utilisation NSUserDefault pour enregistrer les paramètres par défaut dans une application Xamarin.iOS ou une Extension._


La `NSUserDefaults` classe offre un moyen pour iOS, applications et des Extensions pour interagir avec le système de valeurs par défaut à l’échelle du système. À l’aide du système par défaut, l’utilisateur peut configurer un comportement d’une application ou de styles pour répondre à leurs préférences (basés sur la conception de l’application). Par exemple, pour présenter des données dans Visual Studio métrique mesures impériales ou sélectionner un thème de l’interface utilisateur donné.

Lorsqu’il est utilisé avec les groupes d’applications, `NSUserDefaults` fournit également un moyen de communication entre des applications (ou Extensions) dans un groupe donné.

<a name="About-User-Defaults" />

## <a name="about-user-defaults"></a>À propos de l’utilisateur par défaut

Comme indiqué précédemment, les valeurs par défaut de l’utilisateur (`NSUserDefaults`) peut être ajouté à une application (ou Extension) et utilisé pour fournir des options configurables que l’utilisateur final peut modifier pour ajuster l’apparence ou le fonctionnement de l’application lors de l’exécution.

Lorsque votre application s’exécute tout d’abord, `NSUserDefaults` lit les clés et valeurs à partir de la base de données de la valeur par défaut est de l’utilisateur de l’application et les met en cache dans la mémoire pour éviter l’ouverture et la lecture de la base de données chaque fois qu’une valeur est requise. 

> [!IMPORTANT]
> Apple ne sont plus recommande que l’appel de développeur la `Synchronize` méthode pour synchroniser le cache en mémoire avec la base de données directement. Au lieu de cela, elle est appelée automatiquement à intervalles réguliers pour synchroniser le cache en mémoire avec la base de données des valeurs par défaut d’un utilisateur.

La `NSUserDefaults` classe contient plusieurs méthodes pratiques pour lire et écrire des valeurs de préférence pour les types de données courants tels que : chaîne, entier, float, boolean et les URL. Autres types de données peuvent être archivées à l’aide de `NSData`, puis lues ou écrites dans la base de données utilisateur par défaut. Pour plus d’informations, consultez le site d’Apple [préférences et Guide de programmation de paramètres](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i).

<a name="Accessing-the-Shared-NSUserDefaults-Instance" />

## <a name="accessing-the-shared-nsuserdefaults-instance"></a>L’accès à l’Instance partagée NSUserDefaults 

L’Instance par défaut d’utilisateur partagé fournit l’accès à l’utilisateur par défaut pour l’utilisateur actuel de l’appareil. Si l’objet partagé par défaut n’existe pas, elle est créée la première fois, il est accessible et initialisée avec les informations suivantes :

- Un `NSArgumentDomain` comprenant les valeurs par défaut analysés à partir de l’application actuelle.
- Domaine d’identificateur de lot de l’application.
- Un `NSGlobalDomain` comprenant les valeurs par défaut partagés par toutes les applications.
- Un domaine distinct pour chacune des langues par défaut de l’utilisateur.
- Un `NSRegistrationDomain` avec un ensemble de valeurs par défaut temporaires qui peut être modifié par l’application pour garantir la recherche est toujours réussie.

Pour accéder à l’Instance par défaut d’utilisateur partagé, utilisez le code suivant :

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
```

<a name="Accessing-an-App-Group-NSUserDefaults-Instance" />

## <a name="accessing-an-app-group-nsuserdefaults-instance"></a>L’accès à une Instance de NSUserDefaults de groupe de l’application

Comme indiqué précédemment, à l’aide de groupes d’applications, `NSUserDefaults` peut être utilisée pour communiquer entre applications (ou Extensions) dans un groupe donné. Tout d’abord, vous devrez vous assurer que le groupe d’application et les ID d’application requis ont été correctement configurés dans le **certificats, les identificateurs et les profils** section [iOS Dev Center](https://developer.apple.com/devcenter/ios/) et ont été installés dans l’environnement de développement.

Ensuite, vos projets d’application et/ou une Extension doivent prendre les ID d’application valide créé ci-dessus et le `Entitlements.plist` fichier doit être inclus dans le lot d’applications avec les groupes d’application activé et spécifié.

Avec tout ceci en place, le partagé application groupe utilisateur par défaut sont accessibles en utilisant le code suivant :

```csharp
// Get App Group User Defaults
var plist = new NSUserDefaults ("group.com.xamarin.todaysharing", NSUserDefaultsType.SuiteName);
```

Où `group.com.xamarin.todaysharing` est le groupe d’application créé dans **certificats, les identificateurs et les profils** que vous souhaitez accéder. Pour plus d’informations, consultez la [fonctionnalités des applications du groupe](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) documentation.

<a name="Reading-Default-Values" />

## <a name="reading-default-values"></a>La lecture des valeurs par défaut

Une fois que vous avez accédé à la base de données utilisateur par défaut souhaitée, vous pouvez lire les valeurs par défaut à l’aide de paires clé/valeur et plusieurs méthodes d’usage en fonction du type de données en cours de lecture :

- `ArrayForKey` -Retourne un tableau de `NSObjects` pour la valeur de clé donnée.
- `BoolForKey` -Retourne une valeur booléenne pour la clé donnée.
- `DataForKey` -Retourne un `NSData` objet pour la clé donnée.
- `DictionaryForKey` -Retourne un `NSDictionary` pour la clé donnée.
- `DoubleForKey` -Retourne une valeur double pour la clé donnée.
- `FloatForKey` -Retourne une valeur flottante pour la clé donnée.
- `IntForKey` -Retourne une valeur entière pour la clé donnée.
- `StringArrayForKey` -Retourne un tableau de `String` objets à partir de la valeur de clé donnée.
- `StringForKey` -Retourne la valeur de chaîne pour la clé donnée.
- `URLForKey` -Retourne un `NSUrl` valeur pour la clé donnée.

Par exemple, le code suivant lit une valeur booléenne à partir de l’utilisateur par défaut :

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Get value
var useHeader = plist.BoolForKey("UseHeader");

```

<a name="Writing-Default-Values" />

## <a name="writing-default-values"></a>Écriture des valeurs par défaut

Similaire à la lecture des valeurs ci-dessus, une fois que vous avez accédé à l’utilisateur par défaut base de données souhaitée, vous pouvez écrire les valeurs par défaut à l’aide de paires clé/valeur et plusieurs méthodes d’usage en fonction du type de données en cours d’écriture :

- `SetBool` -Écrit la valeur booléenne donnée à la clé donnée.
- `SetDouble` -Écrit la valeur double donnée à la clé donnée.
- `SetFloat` -Écrit la valeur de type float à la clé donnée.
- `SetString` -Écrit la valeur de chaîne donnée à la clé donnée.
- `SetURL` -Écrit l’URL donnée (`NSUrl`) valeur à la clé donnée.

Par exemple, le code suivant est écrit une valeur booléenne à l’utilisateur par défaut :

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Save value
plist.SetBool(useHeader, "UseHeader");
...

```

> [!IMPORTANT]
> Lorsque votre application s’exécute tout d’abord, `NSUserDefaults` lit les clés et valeurs à partir de la base de données de la valeur par défaut est de l’utilisateur de l’application et les met en cache dans la mémoire pour éviter l’ouverture et la lecture de la base de données chaque fois qu’une valeur est requise.



<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a traité la `NSUserDefaults` classe et comment il peut être utilisé pour fournir un ensemble d’options que l’utilisateur final peut utiliser pour configurer votre application Xamarin.iOS. En outre, il couvert à l’aide de groupes d’applications de communiquer entre une extension et son application parente ou entre les applications dans un groupe.


## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [Préférences et paramètres de Guide de programmation](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)
- [NSUserDefaults](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSUserDefaults_Class/#//apple_ref/doc/constant_group/NSUserDefaults_Domains)
