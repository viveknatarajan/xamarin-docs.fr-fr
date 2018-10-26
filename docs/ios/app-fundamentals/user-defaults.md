---
title: Utilisation des valeurs par défaut de l’utilisateur dans Xamarin.iOS
description: Cet article aborde l’utilisation des valeurs NSUserDefaults pour enregistrer les paramètres par défaut dans une application Xamarin iOS ou une extension. Il décrit les valeurs NSUserDefaults à un niveau élevé et explique comment lire et écrire les valeurs.
ms.prod: xamarin
ms.assetid: DAE7FFC4-B8C9-4D9E-886A-9B2388452EEB
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/07/2016
ms.openlocfilehash: 688db534d6c99a8fadb7535f0532f9c1e9564707
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120890"
---
# <a name="working-with-user-defaults-in-xamarinios"></a>Utilisation des valeurs par défaut de l’utilisateur dans Xamarin.iOS

_Cet article aborde l’utilisation des NSUserDefault pour enregistrer les paramètres par défaut dans une application Xamarin.iOS ou une Extension._


Le `NSUserDefaults` classe offre un moyen pour iOS, applications et des Extensions pour interagir par programmation avec le système par défaut de l’échelle du système. En utilisant le système par défaut, l’utilisateur peut configurer de comportement d’une application ou appliquer un style pour répondre à leurs préférences (basés sur la conception de l’application). Par exemple, pour présenter les données de métrique vs Imperial mesures ou sélectionnez un thème de l’interface utilisateur donné.

Lorsqu’il est utilisé avec les groupes d’applications, `NSUserDefaults` fournit également un moyen pour la communication entre applications (ou Extensions) dans un groupe donné.

<a name="About-User-Defaults" />

## <a name="about-user-defaults"></a>À propos des valeurs par défaut de l’utilisateur

Comme indiqué ci-dessus, les valeurs par défaut de l’utilisateur (`NSUserDefaults`) peut être ajouté à une application (ou Extension) et permet de fournir des options configurables que l’utilisateur final peut modifier pour ajuster l’aspect ou le fonctionnement de l’application lors de l’exécution.

Lorsque votre application s’exécute tout d’abord, `NSUserDefaults` lit les clés et valeurs à partir de la base de données de la valeur par défaut est de l’utilisateur de l’application et les met en cache dans la mémoire pour éviter l’ouverture et la lecture de la base de données chaque fois qu’une valeur est requise. 

> [!IMPORTANT]
> Apple ne sont plus recommande que l’appel de développeur le `Synchronize` méthode pour synchroniser le cache en mémoire avec la base de données directement. Au lieu de cela, elle sera appelée automatiquement à intervalles réguliers pour synchroniser le cache en mémoire avec la base de données de valeurs par défaut d’un utilisateur.

Le `NSUserDefaults` classe contient plusieurs méthodes pratiques pour lire et écrire des valeurs de préférence pour les types de données courants tels que : chaîne, entier, float, boolean et URL. Autres types de données peuvent être archivées à l’aide de `NSData`, puis lues ou écrites à la base de données utilisateur par défaut. Pour plus d’informations, consultez le site d’Apple [Guide de programmation de paramètres et des préférences](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i).

<a name="Accessing-the-Shared-NSUserDefaults-Instance" />

## <a name="accessing-the-shared-nsuserdefaults-instance"></a>L’accès à l’Instance de valeurs NSUserDefaults partagées 

L’Instance par défaut d’utilisateur partagé fournit l’accès à l’utilisateur par défaut pour l’utilisateur actuel de l’appareil. Si l’objet partagé par défaut n’existe pas, elle est créée la première fois, il est accessible et initialisée avec les informations suivantes :

- Un `NSArgumentDomain` comprenant les valeurs par défaut analysés à partir de l’application actuelle.
- Domaine de l’identificateur de Bundle de l’application.
- Un `NSGlobalDomain` comprenant les valeurs par défaut partagés par toutes les applications.
- Un domaine distinct pour chacune des langues par défaut de l’utilisateur.
- Un `NSRegistrationDomain` avec un ensemble de valeurs par défaut temporaires qui peut être modifié par l’application pour les recherches sont toujours réussites.

Pour accéder à l’Instance par défaut d’utilisateur partagé, utilisez le code suivant :

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
```

<a name="Accessing-an-App-Group-NSUserDefaults-Instance" />

## <a name="accessing-an-app-group-nsuserdefaults-instance"></a>Accès à une Instance de NSUserDefaults App Group

Comme indiqué ci-dessus, à l’aide de groupes d’applications, `NSUserDefaults` peut être utilisé pour communiquer entre des applications (ou des Extensions) dans un groupe donné. Tout d’abord, vous devrez vous assurer que le groupe de l’application et les ID d’application requis ont été correctement configurés dans le **certificats, identificateurs et profils** section [iOS Dev Center](https://developer.apple.com/devcenter/ios/) et ont été installés dans l’environnement de développement.

Ensuite, vos projets d’application et/ou une Extension doivent disposer d’une instance de l’ID d’application valide créé ci-dessus et le `Entitlements.plist` fichier doit être inclus dans le Bundle d’applications avec les groupes d’application activé et spécifié.

Avec tout cela en place, le partagé application groupe utilisateur par défaut est accessible en utilisant le code suivant :

```csharp
// Get App Group User Defaults
var plist = new NSUserDefaults ("group.com.xamarin.todaysharing", NSUserDefaultsType.SuiteName);
```

Où `group.com.xamarin.todaysharing` est l’App Group créé dans **certificats, identificateurs et profils** que vous souhaitez accéder. Pour plus d’informations, consultez le [application groupe fonctionnalités](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) documentation.

<a name="Reading-Default-Values" />

## <a name="reading-default-values"></a>Lecture des valeurs par défaut

Une fois que vous avez accédé à la base de données utilisateur par défaut souhaitée, vous pouvez lire les valeurs par défaut à l’aide de paires clé/valeur et plusieurs méthodes pratiques en fonction du type de données en cours de lecture :

- `ArrayForKey` -Retourne un tableau de `NSObjects` pour la valeur de clé donnée.
- `BoolForKey` -Retourne une valeur booléenne pour la clé donnée.
- `DataForKey` -Retourne un `NSData` objet pour la clé donnée.
- `DictionaryForKey` -Retourne un `NSDictionary` pour la clé donnée.
- `DoubleForKey` -Retourne une valeur double pour la clé donnée.
- `FloatForKey` -Retourne une valeur float pour la clé donnée.
- `IntForKey` -Retourne une valeur entière pour la clé donnée.
- `StringArrayForKey` -Retourne un tableau de `String` objets à partir de la valeur de clé donnée.
- `StringForKey` -Retourne une valeur de chaîne pour la clé donnée.
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

Similaire à la lecture des valeurs ci-dessus, une fois que vous avez accédé à l’utilisateur par défaut base de données souhaitée, vous pouvez écrire les valeurs par défaut à l’aide de paires clé/valeur et plusieurs méthodes pratiques en fonction du type de données en cours d’écriture :

- `SetBool` -Écrit la valeur booléenne donnée à la clé donnée.
- `SetDouble` -Écrit la valeur double donnée à la clé donnée.
- `SetFloat` -Écrit la valeur de type float à la clé donnée.
- `SetString` -Écrit la valeur de chaîne donnée à la clé donnée.
- `SetURL` -Écrit l’URL donnée (`NSUrl`) valeur à la clé donnée.

Par exemple, le code suivant écririez une valeur booléenne à l’utilisateur par défaut :

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

Cet article a présenté le `NSUserDefaults` classe et comment elle peut être utilisée pour fournir un ensemble d’options que l’utilisateur final peut utiliser pour configurer votre application Xamarin.iOS. En outre, elle couvrait à l’aide de groupes d’applications pour communiquer entre une extension et son application parente ou d’une application dans un groupe.


## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [Préférences et paramètres de Guide de programmation](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)
- [NSUserDefaults](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSUserDefaults_Class/#//apple_ref/doc/constant_group/NSUserDefaults_Domains)
