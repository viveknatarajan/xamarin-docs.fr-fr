---
ms.assetid: 814857C5-D54E-469F-97ED-EE1CAA0156BB
title: Instructions de transfert d’application de bureau
description: Une explication simple montrant comment découpler les formulaires Windows existants ou les applications WPF pour créer des applications multiplateformes à exécuter sur macOS, iOS, Android, ainsi que UWP et Windows 10.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 4bf1dea170bd6b63209693963d54cc2e16163eea
ms.sourcegitcommit: 650458de1d362cd7de174cacef7838f0e74426f3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2019
ms.locfileid: "58071097"
---
# <a name="desktop-app-porting-guidance"></a>Instructions de transfert d’application de bureau

La plupart des codes d’application peuvent être classées parmi les domaines suivants :

* Code d’interface utilisateur (par exemple). Windows et les boutons)
* 3e contrôles tiers (par exemple). graphiques)
* Logique métier (par exemple). règles de validation)
* Accès et stockage de données local
* Accès aux données à distance et services Web

Pour les applications Windows Forms et WPF écrites avec C# (ou Visual Basic.NET) une quantité étonnante de la logique métier, accès aux données locales et code de services web peut être partagée entre les plateformes.

## <a name="net-portability-analyzer"></a>Analyseur de portabilité .NET

Visual Studio 2017 et ultérieur prennent en charge la [Analyseur de portabilité .NET](https://docs.microsoft.com/dotnet/articles/standard/portability-analyzer) ([télécharger pour Windows](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer)) qui peut examiner vos applications existantes et vous indiquent la quantité de code peut être porté « tel quel » à d’autres plateformes. Vous pouvez en savoir plus à partir de ce [vidéo Channel 9](https://channel9.msdn.com/Blogs/Seth-Juarez/A-Brief-Look-at-the-NET-Portability-Analyzer).

Il existe également un outil de ligne de commande peut être téléchargé à partir de [Analyseur de portabilité sur GitHub](https://github.com/Microsoft/dotnet-apiport) et utilisée pour fournir les mêmes rapports.

## <a name="x-of-my-code-is-portable-what-next"></a>« x % de mon code est portable. Et maintenant ? »

J’espère que l’analyseur montre une grande partie de votre code est portable, mais il est certainement sera certaines parties de chaque application qui _ne peut pas_ être déplacés vers d’autres plateformes.

Différents segments de code seront probablement appartiennent à une de ces compartiments, expliqués plus en détail ci-dessous :

* Code portable réutilisables
* Code qui nécessite des modifications
* Code qui n’est pas portable et nécessite une réécriture

### <a name="re-useable-portable-code"></a>Code portable réutilisables

Le code .NET écrit par rapport aux API disponibles sur toutes les plateformes vous pouvez entreprendre inter-plateformes inchangé. Dans l’idéal, vous serez en mesure de déplacer tout ce code dans une bibliothèque de classes Portable, une bibliothèque partagée ou une bibliothèque .NET Standard et à le tester au sein de votre application existante.

Cette bibliothèque partagée peut ensuite être ajoutée aux projets d’application pour d’autres plateformes (par exemple, Android, iOS, Mac OS).

### <a name="code-that-requires-changes"></a>Code qui nécessite des modifications

Certaines API .NET n’est peut-être pas disponible sur toutes les plateformes. Si ces API existe dans votre code, vous devrez réécrire ces sections pour utiliser des API multiplateforme.

Voici quelques exemples utilisent des API de réflexion qui sont disponibles dans .NET 4.6, mais ne sont pas disponibles sur toutes les plates-formes.

Une fois que vous avez ré-écrit le code à l’aide des API portables, il se peut que vous devez être en mesure de ce code dans une bibliothèque partagée et le tester au sein de votre application existante.

### <a name="code-that-isnt-portable-and-requires-a-re-write"></a>Code qui n’est pas portable et nécessite une réécriture

Voici quelques exemples de code qui n’est pas susceptible d’être inter-plateformes :

- **Interface utilisateur** – écrans Windows Forms ou WPF ne peut pas être utilisés dans les projets sur Android ou iOS, par exemple. Votre interface utilisateur doit être réécrites à l’aide de ce [comparaison des contrôles](~/cross-platform/desktop/controls/index.md) en tant que référence.

- **Spécifique à la plateforme stockage** -Code qui s’appuie sur une technologie spécifique à la plateforme (par exemple, une base de données locale de SQL Server Express). Vous devrez réécrire cela à l’aide d’une alternative inter-plateformes (telles que SQLite pour le moteur de base de données).
Certaines opérations de système de fichiers serez peut-être amené à ajuster, étant donné que UWP a légèrement différentes API pour Android et iOS (par ex. certains systèmes de fichiers respectent la casse et d’autres ne sont pas).

- **les composants tiers 3e** – vérifie si des composants tiers 3e dans vos applications sont disponibles sur d’autres plateformes. Certains, tels que les packages NuGet non visuel, ne soient pas disponibles, mais d’autres (en particulier visual contrôles comme les lecteurs de graphiques ou un support)

## <a name="tips-for-making-code-portable"></a>Conseils pour rendre le code portable

- **L’Injection de dépendances** : fournir des implémentations différentes pour chaque plateforme, et

- **Approche en couches** : indique si MVVM, MVC, MVP ou un autre modèle qui vous permet de séparer le code portable à partir du code spécifique à la plateforme.

- **Messagerie** – vous pouvez utiliser la transmission de messages dans votre code à coupler retirer les interactions entre les différentes parties de l’application.
