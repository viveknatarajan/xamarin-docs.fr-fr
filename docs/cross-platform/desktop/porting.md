---
ms.assetid: 814857C5-D54E-469F-97ED-EE1CAA0156BB
title: Conseils de portage d’applications de bureau
description: Une explication simple comment dissocier existant Windows Forms ou les applications WPF pour créer des applications multiplateformes pour s’exécuter sur Mac OS, iOS, Android, ainsi que UWP et Windows 10.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: b9cfad9c046d4f2ad89506f7172a0418e90478f5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781044"
---
# <a name="desktop-app-porting-guidance"></a>Conseils de portage d’applications de bureau

La plupart des codes d’application peuvent être classées parmi les domaines suivants :

* Code de l’interface utilisateur (par exemple). Windows et les boutons)
* contrôles tiers 3e (par exemple). graphiques)
* Logique métier (par exemple). règles de validation)
* Accès et stockage de données local
* Services Web et accès aux données distantes

Pour Windows Forms et WPF applications écrites en c# (ou Visual Basic.NET) une quantité inattendue de la logique métier, les données locales d’accès et le code de services web peut être partagé entre les plateformes.

## <a name="net-portability-analyzer"></a>Analyseur de portabilité .NET

Prise en charge de Visual Studio 2015 et 2017 le [Analyseur de portabilité .NET](https://docs.microsoft.com/en-us/dotnet/articles/standard/portability-analyzer) ([télécharger pour Windows](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer)) qui peut examiner vos applications existantes et vous indiquent la quantité de code peut être porté « tel quel » sur d’autres plateformes . Vous pouvez en savoir plus sur elle à partir de ce [vidéo de Channel 9](https://channel9.msdn.com/Blogs/Seth-Juarez/A-Brief-Look-at-the-NET-Portability-Analyzer).

Il existe également un outil de ligne de commande peut être téléchargé à partir de [Analyseur de portabilité sur GitHub](https://github.com/Microsoft/dotnet-apiport) et utilisé pour fournir les mêmes rapports.

## <a name="x-of-my-code-is-portable-what-next"></a>« x % de mon code est portable. Et maintenant ? »

Nous espérons que l’analyseur affiche une grande partie de votre code est portable, mais il est certainement sera certaines parties de chaque application qui _ne peut pas_ être déplacés vers d’autres plateformes.

Différents segments de code probablement appartient à l’une de ces compartiments expliquées plus en détail ci-dessous :

* Code portable réutilisables
* Code qui requiert des modifications
* Code qui n’est pas portable et requiert une réécriture

### <a name="re-useable-portable-code"></a>Code portable réutilisables

Le code .NET qui est écrit par rapport aux API disponibles sur toutes les plateformes peut être pris inter-plateformes inchangée. Dans l’idéal, vous serez en mesure de déplacer ce code dans une bibliothèque de classes portables, bibliothèque partagée ou bibliothèque Standard de .NET et à le tester dans votre application existante.

Cette bibliothèque partagée peut ensuite être ajoutée aux projets d’application pour d’autres plateformes (par exemple, Android, iOS, Mac OS).

### <a name="code-that-requires-changes"></a>Code qui requiert des modifications

Certaines API .NET n’est peut-être pas disponible sur toutes les plateformes. Si ces API existe dans votre code, vous devez réécrire ces sections pour utiliser des API inter-plateformes.

Voici quelques exemples utilisent des API de réflexion qui sont disponibles dans .NET 4.6, mais ne sont pas disponibles sur toutes les plates-formes.

Une fois que vous avez ré-écrit le code à l’aide des API portable, vous devez être en mesure de ce code dans une bibliothèque partagée du package et de le tester dans votre application existante.

### <a name="code-that-isnt-portable-and-requires-a-re-write"></a>Code qui n’est pas portable et requiert une réécriture

Voici quelques exemples de code qui n’est pas susceptible d’être multiplateforme :

- **Interface utilisateur** – écrans Windows Forms ou WPF ne peut pas être utilisées dans les projets sur Android ou iOS, par exemple. Votre interface utilisateur doit être réécrite à l’aide de ce [comparaison des contrôles](~/cross-platform/desktop/controls/index.md) en tant que référence.

- **Stockage spécifiques de la plate-forme** -Code qui s’appuie sur une technologie spécifique à la plateforme (par exemple, une base de données locale de SQL Server Express). Vous devez réécrire cela à l’aide d’une alternative inter-plateformes (telles que SQLite pour le moteur de base de données).
Certaines opérations de système de fichiers peut-être également être ajusté, étant donné que UWP a légèrement différentes API pour Android et iOS (par exemple). certains systèmes de fichiers respectent la casse et d’autres ne le sont pas).

- **les composants tiers 3e** – vérifie si des composants tiers 3e dans vos applications sont disponibles sur d’autres plateformes. Certaines, tels que les packages NuGet non visuels peuvent être disponibles, mais d’autres (en particulier visual contrôles comme les lecteurs de graphiques ou de support)

## <a name="tips-for-making-code-portable"></a>Conseils pour rendre le code portable

- **Injection de dépendance** : fournir des implémentations différentes pour chaque plateforme, et

- **Approche en couches** : indique si MVVM, MVC, MVP ou certains autres modèles qui vous permet de séparer le code portable à partir du code spécifique à la plateforme.

- **Messagerie** – vous pouvez utiliser la transmission de messages dans votre code à coupler annule les interactions entre les différentes parties de l’application.
